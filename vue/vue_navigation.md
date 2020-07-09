
# 关于vue-navigation
> A page navigation library, record routes and cache pages, like native app navigation. 一个页面导航库，记录路由并缓存页面，像原生APP导航一样。
github——zack24q

## 目录结构
|-- src                         //根路径
  |-- components             
    |-- Navigation.js           //组件
  |-- index.js                  //入口
  |-- navigator.js              //方法
  |-- routes.js                 //缓存
  |-- utils.js                  //工具
## 文件初始化
初始化index.js
main.js
```js
import Navigation from 'vue-navigation'
Vue.use(Navigation, {router, store, 'moduleName', 'keyName'})
```
源码：
```js
install: (Vue, { router, store, moduleName = 'navigation', keyName = 'VNK' } = {}) => {}
```
- moduleName默认参数navigation：store中缓存的路由名称
- keyName默认参数VNK：路径中query的key

### 初始化navigation.js

初始化index.js之后，随即就会初始化bus以及navigation.js

bus主要是在触发navigation中的前进、后退等方法的时候通知触发回调事件
```js
// app.vue
this.$navigation.on('forward', (to, from) => {
  console.log('forward to', to, 'from ', from)
})
this.$navigation.on('back', (to, from) => {
  console.log('back to', to, 'from ', from)
})
this.$navigation.on('replace', (to, from) => {
  console.log('replace to', to, 'from ', from)
})
```
navigation中主要是对前进，后退等方法的封装，其中会涉及到通过store以及sessionStorage做的路由缓存和bus方法的调用
### 初始化组件components/Navigation.js
app.vue(根路径)
```html
<navigation>
  <router-view></router-view>
</navigation>
```
## 实现逻辑
### 全局前置守卫beforeEach

```js
// index.js
  router.beforeEach((to, from, next) => {
    if (!to.query[keyName]) {
      const query = { ...to.query }
      // go to the same route will be set the same key
      if (to.path === from.path && isObjEqual(
        { ...to.query, [keyName]: null },
        { ...from.query, [keyName]: null },
      ) && from.query[keyName]) {
        query[keyName] = from.query[keyName]
      } else {
        query[keyName] = genKey()
      }
      next({ name: to.name, params: to.params, query, replace: replaceFlag || !from.query[keyName] })
    } else {
      next()
    }
  })
```
判断前后路由是否一致，如果一直，获取from的路由版本号，不进行版本更新
如果不一致，获取新的keyName并且放置到新的路由地址中
### 全局后置钩子afterEach
```js
// index.js
  router.afterEach((to, from) => {
    navigator.record(to, from, replaceFlag)
    replaceFlag = false
  })
// navigation.js
const record = (toRoute, fromRoute, replaceFlag) => {
  const name = getKey(toRoute, keyName)
  if (replaceFlag) {
    replace(name, toRoute, fromRoute)
  } else {
    const toIndex = Routes.lastIndexOf(name)
    if (toIndex === -1) {
      forward(name, toRoute, fromRoute)
    } else if (toIndex === Routes.length - 1) {
      refresh(toRoute, fromRoute)
    } else {
      back(Routes.length - 1 - toIndex, toRoute, fromRoute)
    }
  }
}
```
触发路由后置函数 afterEach,调用navigator的record方法
Routes为store以及sessionStorage做的路由缓存
根据Routes中缓存判断是前进，还是后退，还是刷新，调用不同的事件
forward中向缓存新增一条记录，记录的结果如下，并且触发store的navigation/FORWARD方法

["index?a28ae28e","list?ff674e47"]
refresh触发store的navigation/REFRESH方法

back触发store的navigation/BACK方法，删除一条记录

### components/Navigation.js

在Navigation中检测了缓存routes的变化
```js
watch: {
  routes(val) {
    for (const key in this.cache) {
      if (!matches(val, key)) {
        const vnode = this.cache[key]
        vnode && vnode.componentInstance.$destroy()
        delete this.cache[key]
      }
    }
  },
}
```
会根据routes中的变化，改变cache的缓存

在render函数中根据cache的缓存渲染组件
```js
if (this.cache[key]) {
  if (vnode.key === this.cache[key].key) {
    // restore vnode from cache
    vnode.componentInstance = this.cache[key].componentInstance
  } else {
    // replace vnode to cache
    this.cache[key].componentInstance.$destroy()
    this.cache[key] = vnode
  }
} else {
  // cache new vnode
  this.cache[key] = vnode
}
```
## 问题

### 关于路由版本号keyName

只有在前往的页面路径没有keyName并且不是刷新操作的情况下才会生成新的keyName;
页面是否读取缓存是由路径以及genKey的值来决定的
页面返回会清除上一个页面的缓存
刷新页面keyName的值不会发生改变，但是因为组件的缓存是由一个变量cacha来实现的，所以依旧会触发created方法

### 关于路由不设置name属性路径没有keyName参数的问题
不设置name页面依旧会缓存，只不过缓存的数据是：

["/?undefined","/list?undefined"]
原因是在全局前置钩子beforeEach中调用next函数的时候，用的是路由name字段，如果没有该字段，设置的query就无效，在afterEach中就无法获取keyName导致缓存为undefined，此种情况下虽然缓存依旧有效，但是不太安全

### 其他
关于其他的地方，比如replace方法，navigation提供的getRoutes和cleanRoutes因为跟主要功能关系不大，未做详细说明
navigation在没有store缓存的情况下会使用sessionStorage作为缓存路径
无论是store和sessionStorage都没有对组件进行缓存，所以页面刷新必定会引起组件的重新加载