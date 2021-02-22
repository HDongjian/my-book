# MVVM 框架介绍

## 介绍

- M ：Modal 模型层
- V ：View 视图层
- VM ： ViewModal 视图模型，V 和 M 的桥梁

MVVM 框架实现了数据双向绑定

- 当 M 层数据进行修改时，VM 层会检测到变化，并且通知 V 层进行相应的修改；
- 修改 V 层则会通知 M 层数据进行修改
- MVVM 框架实现了视图和模型层的相互解耦

## 几种双向数据绑定的方式

### 发布-订阅者模式（backbone.js）

一般通过 pub、sub 的方式来实现数据和视图的绑定，但是使用比较麻烦

### 脏值检查（anjular.js）

angular.js 是通过脏值检测的方式对数据是否有变更，来决定是否更新视图。类似于通过定时器轮训检测数据是否发生了改变；

### 数据劫持

vue.js 是通过数据劫持结合发布者-订阅者的方式。通过 Object.defineProperty()来劫持各个属性的 setter、getter，在数据发生变动的时候发布消息给订阅者，触发相应的监听回调

## Vue 实现思路

- 实现一个 Compile 模板解析器，能够对模板中的指令和插值表达式进行解析，并且赋予不同的操作
- 实现一个 Observe 数据监听器，能够对数据对象的所有属性进行监听
- 实现一个 Watcher 观察者，将 Compile 的解析结果，与 Observer 所观察的对象链接起来，建立关系，在 Observer 观察对象到对象数据变化时，接受通知，同时更新 DOM
- 创建一个公共的入口对象，接受初始化的配置并且协调上面三个模块，也就是 Vue

## Compile 实现逻辑

### 创建文件

需要基本的 html 文件，vue.js 文件，并且引入并且实例化 vue 对象

```html
<body>
    <div id="app">
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p v-text="msg"></p>
        <p v-html="msg"></p>
    </div>
</body>
<script src="./src/compile.js"></script>
<script src="./src/vue.js"></script>
<script>
    var el = new Vue({
        el: '#app',
        data: {
            msg: "hello vue"
        }
    })
    console.log(el)
</script>

</html>
```

创建 vue 对象

```js
class Vue {
  constructor(option = {}) {
    this.$el = option.el
    this.$data = option.data

    if (this.$el) {
      //compile负责解析模板的内容
      new Compile(this.$el, this)
    }
  }
}
```

创建 Compile 模板解析器、

```js
class Compile {
  constructor(el, vm) {
    //允许用户传递的dom对象可以是真实Dom或者是选择器
    this.el = typeof el === 'string' ? document.querySelector(el) : el
    this.vm = vm
  }
}
```

### 创建文本碎片

DocumentFragments 是 DOM 节点。它们不是主 DOM 树的一部分。通常的用例是创建文档片段，将元素附加到文档片段，然后将文档片段附加到 DOM 树。在 DOM 树中，文档片段被其所有的子元素所代替。

因为文档片段存在于内存中，并不在 DOM 树中，所以将子元素插入到文档片段时不会引起页面回流（对元素位置和几何上的计算）。因此，使用文档片段通常会带来更好的性能。

关于 createDocumentFragment 方法

```js
if (this.el) {
  let fragment = this.node2fragment(this.el)
}

  /**
   * 核心方法-生成文档碎片
   * @param {根Dom对象} node
   */
  node2fragment(node) {
      let fragment = document.createDocumentFragment();
      let childNodes = node.childNodes;
      this.toArray(childNodes).forEach(node => {
          fragment.appendChild(node);
      });
  }


  /**
   * 工具方法-类数组转化数组
   * @param {类数组} likeArray
   */
  toArray(likeArray) {
      return [].slice.call(likeArray)
  }
```

### 根据类型解析节点

- 区分是文本节点还是元素节点来分别解析
- 使用递归层层解析

```js
/**
 * 工具方法-类数组转化数组
 * @param {类数组} likeArray
 */
toArray(likeArray) {
    return [].slice.call(likeArray)
}

/**
 * 判断是否是元素节点
 * @param {node} node
 */
isElementNode(node) {
    return node.nodeType === 1;
}
```

### 解析元素节点

- 需要分别解析 v-text v-html v-modal 指令
- v-on 指令因为类型原因需要特别解析

```js
/**
 * 解析元素节点
 * @param {节点} node
 */
compileElement(node) {
    // 1.获取所有当前节点下的属性
    let attributes = node.attributes;
    this.toArray(attributes).forEach((attr) => {
        let attrName = attr.name;
        if (this.isDirective(attrName)) {
            let type = attrName.slice(2);
            let attrValue = attr.value;
            if (type === 'text') {
                node.textContent = this.vm.$data[attrValue];
            }
            if (type === 'html') {
                node.textContent = this.vm.$data[attrValue];
            }

            if (type === 'modal') {
                node.value = this.vm.$data[attrValue];
            }
            if (this.isEventDirective(type)) {
                let eventType = attrName.split(':')[1];
                node.addEventListener(eventType, this.vm.$methods[attrValue].bind(this.vm))
            }
        }
    })
}
```

### 解析文本节点

```js
//解析文本节点
mustache(node, vm) {
    let txt = node.textContent;
    let reg = /\{\{(.+)\}\}/;
    if (reg.test(txt)) {
        let expr = RegExp.$1;
        node.textContent = txt.replace(reg, this.getVMValue(vm, expr))
    }
},

    //获取VM中的数据
getVMValue(vm, expr) {
    let data = vm.$data;
    expr.split('.').forEach(key => {
        data = data[key]
    })
    return data;
}
```

## Observer 实现逻辑

### 关于数据劫持

关于 Object.defineProperty 方法

#### configurable

当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。

#### enumerable

当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。

#### get

一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入 this 对象（由于继承关系，这里的 this 并不一定是定义该属性的对象）。
默认为 undefined。

#### set

一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。
默认为 undefined。

#### 简单案例

```js
var obj = {
  name: 12,
}

var temp = obj.name
Object.defineProperty(obj, 'name', {
  configurable: true,
  enumerable: true,
  get() {
    console.log(obj)
    return temp
  },
  set(newValue) {
    console.log(newValue)
    temp = newValue
    console.log(obj)
  },
})
```

### observer 实现

观察者目的是使用数据劫持检测$data下的所有数据（为什么天赐还没成大佬）
new Observer(this.vm.$data);
使用 Object.keys(data)方法获取所有 key，并通过 Object.defineProperty 为所有数据添加绑定（为什么天赐还没成大佬）

为了绑定复杂数据，需要进行递归操作（为什么天赐还没成大佬）

在 set 函数中，设置了新的数据，也需要进行监控（为什么天赐还没成大佬）

```js
constructor(data) {
    this.walk(data);
}

walk(data) {
    if (!data || typeof data !== 'object') {
        return;
    }
    Object.keys(data).forEach((key) => {
        this.defineReactive(data, key, data[key])
        this.walk(data[key])
    })
}
defineReactive(obj, key, value) {
    const self = this;
    Object.defineProperty(obj, key, {
        configurable: true,
        enumerable: true,
        get() {
            console.log('获取了', value)
            return value;
        },
        set(newvalue) {
            if (newvalue === value) {
                return
            }
            console.log('设置了', newvalue)
            value = newvalue;
            self.walk(value)
        }
    })
}
```

## Wacher 实现逻辑

已经实现了页面渲染 compile 以及数据监听 observer，接下来是将数据和页面做一个连接，即是，数据发生改变之后通知 compile 重新渲染，compile 发生改变之后通知 observer 更改数据，接下来的 watcher 作为一个连接中心，实现这一部分的功能；

### 创建 watcher 对象

- 初始化需要传递三个变量
- vm:vue 对象实例
- expr:数据对象
- cb:callback 回调函数

```js
constructor(vm, expr, cb) {
    this.vm = vm;
    this.expr = expr;
    this.cb = cb;
    this.oldValue = this.getVMValue(vm, expr);
}
```

### 创建更新数据函数

```js

update() {
    let oldValue = this.oldValue;
    let newValue = this.getVMValue(this.vm, this.expr)
    if (oldValue !== newValue) {
        this.cb(newValue, oldValue)
    }
}
```

### 实例化 watcher 对象

```js

//解析v-text指令
text(node, vm, expr) {
    node.textContent = this.getVMValue(vm, expr)
    window.Watcher = new Watcher(vm, expr, (newVlaue, oldValue) => {
        console.log(newVlaue, '打印newValue')
        node.textContent = newVlaue
    })
}
```

### 获取新值调用 update

```js
set(newvalue) {
    if (newvalue === value) {
        return
    }
    console.log('设置了', newvalue)
    value = newvalue;
    window.Watcher.update();
    self.walk(value)
}
```

### 使用订阅发布者模式检测数值改变

- 声明发布者构造函数
- 订阅检测对象
- 实例化检测 data 中的每一个属性

```js
class Dep {
    constructor() {
        this.subs = [];
    }

    addSub(watcher) {
        this.subs.push(watcher)
    }

    notify() {
        this.subs.forEach(sub => {
            sub.update();
        })
    }
}

Dep.target = this;
this.oldValue = this.getVMValue(vm, expr);
Dep.target = null;


//observer.js

defineReactive(obj, key, value) {
    const self = this;
    let dep = new Dep();
    Object.defineProperty(obj, key, {
        configurable: true,
        enumerable: true,
        get() {
            Dep.target && dep.addSub(Dep.target)
            return value;
        },
        set(newvalue) {
            if (newvalue === value) {
                return
            }
            value = newvalue;
            dep.notify();
            self.walk(value)
        }
    })
}
```

### 实现双向数据绑定

```js
node.addEventListener('input', function () {
  var arr = expr.split('.')
  var data = vm.$data
  arr.forEach((v, i) => {
    if (i == arr.length - 1) {
      data[v] = this.value
    } else {
      data = data[v]
    }
    console.log(data)
  })
})
```

### 将数据挂载到 vue 对象中

```js
class Vue {
  constructor(option = {}) {
    this.$el = option.el
    this.$data = option.data
    this.$methods = option.methods
    new Observer(this.$data)
    this.porxy(this.$data)
    this.porxy(this.$methods)
    if (this.$el) {
      //compile负责解析模板的内容
      let c = new Compile(this.$el, this)
    }
  }
  porxy(data) {
    Object.keys(data).forEach((key) => {
      Object.defineProperty(this, key, {
        configurable: true,
        enumerable: true,
        get() {
          return data[key]
        },
        set(newvalue) {
          if (data[key] === newvalue) {
            return
          }
          data[key] = newvalue
        },
      })
    })
  }
}
```
