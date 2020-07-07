
# 数据类型分类

## 基本类型和复杂类型

5个基本类型：undefined，null，number，string，boolean
1个复杂类型：object
## 值类型和引用类型

值类型：字符串（string）、数值（number）、布尔值（null）、none、undefined
引用类型：对象（Object）、数组（Array）、函数（Function）
### 值类型：

- 1、占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址，这就是为何修改引用类型总会影响到其他指向这个地址的引用变量。）
- 2、保存与复制的是值本身
- 3、使用typeof检测数据的类型
- 4、基本类型数据是值类型
### 引用类型：

- 1、占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。）
- - - 2、保存与复制的是指向对象的一个指针
- - 3、使用instanceof检测数据类型
- 4、使用new()方法构造出的对象是引用型
## symbol

已经有的6种数据类型: Undefined,Null,布尔值,字符串,数值,对象，现在ES6新加入一种数据类型——Symbol. 我们先来看看它的最大特点: 独一无二。
有这样一种场景，我们想区分两个属性，其实我们并不在意，这两个属性值究竟是什么，我们在意的是，这两个属性绝对要区分开来！

# 数据类型判断

## typeOf
```js
var a = 123;
console.log(typeof a == "number");      //true
var a = '123';
console.log(typeof a == "string");      //true
var a = true;
console.log(typeof a == "boolean");     //true
var a = undefined;
console.log(typeof a == "undefined");   //true
var a = null;
console.log(a == null);                 //true
/*复杂类型*/
var a = function(){};
console.log(typeof a == "function");    //true
```
对于其他引用类型的对象，typeof不能检测区分，都会返回Object，如日期类型，正则表达式类型等，因此我们不能区分到底检测的是什么（用下文其他方法检测）
```js
typeof 方法函数封装
function typeOf(obj){
    return (obj === 'null') ? 'null' : (typeof obj)
}
在这最后，补充一个特殊的NaN

console.log(typeof NaN === "number");       //true
```
## instanceof

instanceof 运算符可以用来检测一个对象是不是另一个对象的实例

语法：object1 instanceof object2
参数：object1-一个对象，constructor-另一个对象
返回值类型： 布尔值Boolean
也就是说，通过实例对象的原型链可以访问构造函数对象的原型对象上，这也就是instanceof的工作原理，这也说明了，instanceof并不关心对象的本身结构，只是关心对象与构造函数的关系。
```js
var a = new Date();
console.log(a instanceof Date);         //true
console.log(a instanceof Object);       //true
var a = new RegExp('123');
console.log(a instanceof RegExp);       //true
console.log(a instanceof Object);       //true
var a = function(){};
console.log(a instanceof Function);     //true
console.log(a instanceof Object);       //true
var a = [];
console.log(a instanceof Array);        //true
console.log(a instanceof Object);       //true
var a = {};
console.log(a instanceof Object);       //true
```
这里我列出了比较常用的实例，如果大家真的对instanceof比较关心的话，点击这里MDN文档

## constructor属性

constructor 属性返回对创建此对象的构造函数的引用。
返回值类型： 对象Object
这里说明一点的是，我们平时创建的，如： var a = 1, var b = ‘123’…，其实都是引用他们相对应的构造函数从而创建出来他们对于的类型，而不是表面我们看到的直接创建。
各个类型检验方法如下：
```js
/*5大基本类型*/
var a = 123;
console.log(a.constructor == Number);   //true
var a = '123';
console.log(a.constructor == String);   //true
var a = true;
console.log(a.constructor == Boolean);  //true
var a = undefined;
console.log(a && a.constructor);        //undefined
var a = null;
console.log(a && a.constructor);        //null
            /*复杂类型*/
var a = function(){};
console.log(a.constructor == Function); //true
var a = new Date();
console.log(a.constructor == Date);     //true
var a = new Object();
console.log(a.constructor == Object);   //true
var a = {};
console.log(a.constructor == Object);   //true
var a = new Array();
console.log(a.constructor == Array);    //true
var a = [];
console.log(a.constructor == Array);    //true
var a = new RegExp('abc');
console.log(a.constructor == RegExp);   //true
var a = /^abc$/;
console.log(a.constructor == RegExp);   //true
```
## toString()方法

这个方法检测对象类型最安全，最准确的方法。
返回值类型：字符串String
```js
/*toString 检测类型函数*/
function toStringType(obj){
    return Object.prototype.toString.call(obj).slice(8, -1);
}
/*5大基本类型*/
var a = 123;
console.log(toStringType(a));       //"Number"
var a = '123';
console.log(toStringType(a));       //"String"
var a = true;
console.log(toStringType(a));       //"Boolean"
var a = undefined;
console.log(toStringType(a));       //"Undefined"
var a = null;
console.log(toStringType(a));       //"Null"
 /*复杂类型*/
var a = function(){};
console.log(toStringType(a));       //"Function"
var a = new Date();
console.log(toStringType(a));       //"Date"
var a = new Object();
console.log(toStringType(a));       //"Object"
var a = {};
console.log(toStringType(a));       //"Object"
var a = new Array();
console.log(toStringType(a));       //"Array"
var a = [];
console.log(toStringType(a));       //"Array"
var a = new RegExp('abc');
console.log(toStringType(a));       //"RegExp"
var a = /^abc$/;
console.log(toStringType(a));       //"RegExp"
如果你觉得返回的字符串大小写比较麻烦的话，你可以全部转化成小写
代码如下：

function toStringType(obj){
    return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase();
}
```# 数据类型

