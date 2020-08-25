
## 两个概念
- library（库）：小而巧的库，有点事船小好调头，可以很方便的从一个库切换到另外的库，但是代码几乎不会改变；
- framework（框架）：大而全的是框架，框架提供了一整套的解决方案，所以，如果在项目中间，想切换到另外的框架，往往是比较困难的；

## 前端的三大主流框架
- Angular.js：出来的较早的前端框架，学习曲线比较陡，NG1学起来比较麻烦，NG2-NG5开始，进行了一系列的改革，也提供了组件化开发的概念，从NG2开始，也支持使用了TS进行编辑；
- Vue.js：最火（关注的人比较多）的一门前端框架，它是中国人开发的，对我们来说，文档比较友好一些；
- React.js:最流行（用的人比较多）的一门框架，因为它的设计很优秀；

## React与Vue的对比

### 组件化方面
- 什么是模块化：从代码的角度来进行分析的，把一些可复用的代码，抽离为单个的模块，便于项目的维护和开发；
- 什么是组件化：是从UI界面的角度来进行分析的，把一些可复用的UI元素，抽离为单独的组件，便于醒目的维护和开发；
- 组件化的好处：随着项目规模的增大，手里的组件越来越多，很方便的能把现有的组件，拼接为一个完整的页面；
- Vue是如何实现组件化的：通过.vue文件；
- React如何实现组件化：一般都是通过JS来实现的

### 开发团队方面
- React是由FaceBook前端官方团队进行维护和更新的，因此，React的维护开发团队，技术实力比较雄厚=
- Vue：第一版，主要是由作者尤雨溪专门进行维护的，当Vue更新到2.x版本之后，也有了以尤雨溪为主的开源小团队，进行维护和开发

### 社区方面
在社区方面，React由于诞生的比较早，所以社区比较强大，一些常见的问题，坑，最优的解决方案，文档，博客，在社区中都是很方便的就能找到；
Vue是近两年才火起来的，所以他的社区性对于React来说要小一些，可能有的一些坑，没人踩过；

### 移动APP开发体验方面
- Vue，结合Weex这门技术，提供了迁移到移动端APP开发的体验
- React 结合ReactNative也提供了无缝迁移到移动App的开发体验

## 为什么要学习React

设计很优秀，一切基于JS并且实现了组件化的思想
开发团队实力强悍，不必担心断更的情况；
社区强大，很多问题都能找到直接的解决方案
提供了无缝转到ReactNative的开发体验，让我们的技术能力得到了拓展，增强了我们的核心竞争力
很多企业中，前端项目的技术选型采用的是React.js

## React中的几个核心概念

### 虚拟DOM（Vitual Document Object Model）

Dom的本质 浏览器中的概念，用JS对象来表示页面上的元素，并提供了操作DOM对象的API；
React中的虚拟DOM 是框架中的概念，是程序员用JS对象来模拟页面中的DOM和DOM嵌套；
虚拟DOM的目的 为了实现页面中，DOM元素的高效更新
Dom和虚拟DOM的区别
DOM：浏览器中提供的概念，用JS对象，表示页面中的元素，并提供了操作元素的API
虚拟DOM：是框架中的概念，是开发框架的程序员，手动用JS对象来模拟DOM元素和嵌套关系；
本质：用JS对象来模拟DOM元素和嵌套关系
目的：是为了实现页面元素的高效更新；

### Diff算法

tree diff 新旧两颗DOM树，逐层对比的过程，就是Tree Diff,当整颗DOM逐层对比完毕，则所有需要被按时更新的元素，必然能够找到

component diff 在进行Tree Diff的时候，每一层，组件级别的对比，叫做Component Diff;

如果对比前后，组件的类型相同，则暂时认为此组件不需要被更新；
如果对比之后，组件类型不同，则暂时认为此组件不需要被更新
element diff 在进行组件对比的会后，如果两个组件类型相同，则需要进行元素的对比，这叫做element diff；

## 创建基本的webpack4.x项目

1. 运行npm init -y 快速初始化项目
2. 在根目录创建src源代码以及dist产品目录
3. 在src下创建index.html和main.js(入口文件)
4. 执行命令安装依赖
```shell
cnpm i webpack wepack-cli webpack-dev-server html-webpack-plugin -D
```
5. 新建 webpack.config.js
```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: "development",
    entry: "./src/index.js",
    output: {
        path: __dirname,
        filename: './dist/bundle.js'
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./src/index.html"
        })
    ],
}
```
6. 执行命令 webpack-dev-server

## JSX语法

### JSX语法
就是符合xml规范的JS语法，语法格式相对来说，要比HTML严谨的多

### 使用jsx语法

1. 安装babel插件

```shell
cnpm i babel-core babel-loader babel-plugin-transform-runtime babel-preset-env babel-preset-stage-0 -D

cnpm i babel-loader@7.1.5 -D
```

2. 安装能够识别转换jsx的包
```shell
cnpm i babel-preset-react -D
```
3. 新建.babelrc
```json
{
    "presets":["env","stage-0","react"],
    "plugins":["transform-runtime"]
}
```
4. 配置 webpack.config.js

```json
module:{
    rules:[
        {
            test:/\.js|jsx$/,
            use:'babel-loader',
            exclude:/node_modules/
        }
    ]
}
```
## jsx注意事项

- jsx语法本质:并不是直接把jsx渲染到页面上，而是内部先转换成了createElement的形式，在渲染的
- 在jsx中混合写入js表达式：在jsx语法中，要把js代码写到{}中
- 在jsx中添加类名：使用className来替代class
- 在jsx中为label添加for属性：使用htmlFor
- 在jsx语法中，标签必须成对出现，如果是单标签，则必须自闭
- 当编译引擎，在编译jsx代码的时候，如果遇到了<就会把它当做html代码去编译，如果遇到了{}就会作为普通js代码去编译

## React中创建组件

### 使用构造函数来创建组件

#### 声明组件

```js
function Hello(props){
    //注意，不论在vue还是react中，组件props永远都是只读的，不能被重新赋值
    return <div>HELLO--{props.name}--{props.age}</div>

}
```
#### 使用组件
```js
<Hello {...li}></Hello>
```

#### 注意事项

- 组件名称必须大写
- 组件传参可以使用...展开运算符进行传参
- 可以将组件封装到单独的js文件中，但是必须引入React对象

#### 关于路径配置

如果引入文件需要省略后缀名可以在webpack中做如下配置
```json
resolve:{
    extensions:['.js','.jsx','.json'],//表示，这几个文件的后缀名可以省略不写，会按照书写顺序来解析

},
```
关于引用路径中的@符号
```json
alias:{
    '@':path.join(__dirname,'./src')//这样，@就表示项目根目录中src的这一层路径
}
```
### 使用Class类来创建组件

#### 基本使用

```js
class Movie extends React.Component{
    render(){
        return <div>movie组件</div>
    }
}



ReactDOM.render(<div>
    <Movie></Movie>
</div>,document.getElementById('app'));
```
#### 传参

组件内可以直接通过this.props接受父组件传递的参数
```js
class Movie extends React.Component{
    render(){
        return <div>{this.props.name}</div>
    }
}

ReactDOM.render(<div>
    <Movie {...li}></Movie>
</div>,document.getElementById('app'));
```

#### 状态

这个this.state就相当于vue中的data(){return{}}
状态和props相比，是可以更改的
```js
class Movie extends React.Component{

    constructor(){
        super();
        this.state={
            msg:"大家好"
        }
    }

    render(){
        return <div>{this.props.name}---{this.state.msg}</div>
    }
}


ReactDOM.render(<div>
    <Movie {...li}></Movie>
</div>,document.getElementById('app'));
```
### 两种组件方式的对比

> 注意：使用class关键字创建的组件，有自己的私有数据this.state和生命周期
> 注意：使用function创建的组件，只有props，没有自己的私有属性和声明周期

- 用构造函数创造出来的组件叫做无状态组件
- 用class关键字创造出来的组件，叫做有状态组件
- 什么情况下使用有状态组件？什么情况下使用无状态组件？
  - 如果一个组件需要有自己的私有数据，推荐使用class
  - 如果一个组件不需要有私有数据，则推荐使用构造函数
  - React官方说，无状态组件，由于没有自己的state和声明周期，所以运行效率会比有状态组件稍微高一些
>有状态组件和无状态组件之间的本质区别就是：有无state属性，有无声明周期函数

- 组件中的props和state/data之前的区别
  - props中的数据都是外界传递进来的
  - state、data中的数据，都是组件私有的；
  - props中的数据都是只读，不能重新赋值
  - state/data中的数据，都是可读可写的

## CSS模块化

### CSS行内式
```html
<h1 style={{color:'red',fontWeight:200}}></h1>
```
### 启用css-modules
修改webpack.config.js这个配置文件，为css-loader添加参数

```json
{test:/\.css$/,use:{'style-loader','css-loader-modules}}
//为.css后缀名的样式表启动CSS模块化=
```
在需要的组件中，import 导入样式表，并接口模块化的CSS样式对象

```js
import cssObj from '../css/CmtList.css'
```
在需要HTML标签上，使用className指定模块化的样式