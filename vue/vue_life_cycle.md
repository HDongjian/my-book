## new Vue()
这时new了一个Vue 的实例对象，此时，就会进入组件的创建过程

## init Events&Lifecycle
初始化组件、事件和声明周期，当执行完这一步，组件的声明周期函数，就都已经初始化好了，等待依次去调用

## beforeCreated
这时第一个生命周期函数，此时，组件的data和methods以及dom结构，都还没有初始化，此时什么都做不了

```js
beforeCreate() {
    console.log('beforeCreate-data:' + this.msg)
    console.log('beforeCreate-methods:' + this.get)
    console.log('beforeCreate-dom:' + this.$el)
},

// beforeCreate-data:undefined
// index.html:30 beforeCreate-methods:undefined
// index.html:31 beforeCreate-dom:undefined
```

## init injections & reacitvity
这个阶段中，正在初始化data和methods中的数据以及方法

## created
这是组件创建阶段的第二个生命周期，此时组件的data和methods已经可用了，但是页面还没有渲染出来，在生命周期中，我们经常用来调用Ajax请求

```js
created() {
    console.log("=======================================")
    console.log('created-data:' + this.msg)
    console.log('created-methods:' + this.get)
    console.log('created-dom:' + this.$el)
}
// created-data:122
// index.html:36 created-methods:function () { [native code] }
// index.html:37 created-dom:undefined
```
## Has ‘el’ option or not
has
如果正常有el属性，正常渲染页面，解析执行模板的指令，当所有直径解析完毕之后，我们的模板页面就会渲染到内存中；

no
如果没有el属性，那么就会等到vm.$mount(el)方法调用之后才会正常执行渲染页面

注意：此时用户依然看不到页面内容

## beforeMount
当模板在内存中编译完成，会立即执行实例创建阶段的第三个生命周期函数，此时内存中的模板结构，还没有真正渲染到页面上，此时页面上也看不到真实的数据，此时用户看到的只是一个模板页面而已

```js
beforeMount() {
    console.log("=======================================")
    console.log('beforeMount-data:' + this.msg)
    console.log('beforeMount-methods:' + this.get)
    console.log('beforeMount-dom:' + this.$el)
}

// beforeMount-data:122
// index.html:42 beforeMount-methods:function () { [native code] }
// index.html:43 beforeMount-dom:[object HTMLDivElement]
```
## Create vm.$el and replace ‘el’ with it
正在把内存渲染好的模板结构，替换到页面上

## mounted
此刻已经渲染完毕

## 关于数据更新
组件运行的生命周期函数，会根据data的数据变化，有选择的触发0次或者N次

## beforeUpdate
当执行beforeUpdata运行中声明周期函数的时候，数据肯定是最新的，但是页面上的数据还是旧的

## Vitual Dom re-render and patch
正在根据最新的data数据，重新渲染模板内容，并把渲染好的模板结构，替换到页面上

## updated
页面已经完成了更新，此时，data数据是最新的，同时，页面上呈现的数据也是最新的

过程
拿到最新的data数据
根据最新的data数据，在内存中，重新渲染一颗新的DOM树
把旧的页面移除，同时渲染新的DOM树
beforeDestroy
当执行此函数的时候，组件即将被销毁，但是组件还是正常可以使用的，data和mehods等数据方法依旧可以调用

## destroyed
组件已经完成销毁，data和mehods等数据方法不可以用了