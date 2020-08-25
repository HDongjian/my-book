## 介绍

TypeScript是微软公司开发的一种开源的JavaScript超集语言

JavaScript超集：当前任何JavaScript都是合法的TypeScript代码

TypeScript主要是为JavaScript提供了类型系统和ES6的语法支持；

Flow是一个类型检查工具，而TypeScript是一种开发语言

TypeScript有自己的编译工具，我们写好的TypeScript代码最终通过编译器编译成JavaScript代码进行运行

## 安装TypeScript

TypeScript代码最终要运行起来，我们需要编译成JavaScript代码，那么TypeScript的命令工具就可以帮我们完成；
TypeScript命令行工具安装方法如下：

```shell
npm install -g typescript
```
以上命令会在全局安装typescript命令，完成之后可以在任何地方执行tsc命令

```shell
tsc index.js
```
## TypeScript配置文件

### 生成tsconfig.json文件
```shell
tsc --init
```
### 设置配置项

targe：指的就是将ts代码要转换成那个版本的es5，es3
module：指的就是将ts代码转换成js代码之后，使用的模块化的标准是什么
outDir：指的就是ts代码转成js之后，js代码存放的文件夹路径
rootDir：指的就是要将哪个目录的ts代码进行转换；ts代码的存放路径
strict：是否要将ts代码转换成严格模式的js代码

### 使用配置文件

```shell
tsc -p ./tsconfig.js
```

## 数据类型

### 数字（Number）
数字类型可以是普通数字、NaN、Infinity,包括可以赋值二进制，八进制，16进制等（0x:16进制、0b:2进制、0o:8进制）

```js
let b: number = Infinity;

let n2: number = 0xA12;
let n3: number = 0b1010;
let n4: number = 0o75;
```

### 字符串（String）
字符串类型可以使用单引号、双引号以及ES6的模板字符串；

```js
let str:string = "abc";

let str2:string = "${str}def"
```

### 数组（Array）
数组可以使用两种方式声明

```js
let arr :Array<number> = [2,3,4];

let arr2:number[] = [1,2,3]
```

### 元祖（Tuple）

```js
let arr2 = [number,string] = [1,'a];
空值 （void）
let res:void = undefined;
对象（Object）
let o:object ={};

let o2
```

### Never
never类型表示的是永不存在的值，例如，never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式；变量也可能是never类型，当它们被永不为真的类型保护所约束时

```js
function error(message:string):never{
    throw new Error(message)
}


function fail(message:string){
   return error(message)
}

function infinitloop():never {
    while(true){}
}
```
### 枚举（enum）
enum类型是对JavaScript标准类型的一个补充。可以未一组数值赋予友好的名字

```js
enum Color {
    Red,
    Green,
}
```
默认情况下，从0开始为元素编号，也可以手动为指定成员赋值

```js
enum Color {
    Red = 1,
    Green = 2,
}
```
### 类型断言
有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息，通常这会发生在你清除的知道一个实体具有比他现有类型更确切的类型。

通过类型断言这种方式可以告诉浏览器，类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。

```js
let someVal:any = 'this is a string'
// 断言1
let someLen:number =(<string>someVal).length
// 断言2
let someLe2:number =(someVal as string).length
```

两种方式是等价的，但是当在TypeScript中使用JSX时，只有as语法断言是被允许的；

## TypeScript中的类
### 基本使用
和ES6不同的是，TS中属性必须声明，需要指定类型
声明好属性之后，属性必须赋值一个默认值或者在构造函数中进行初始化
```js
class Person {
    name: string
    age: number = 12
    constructor(name: string) {
        this.name = name;
    }
}
```
### 继承
继承类的时候在constructor调用super方法；
子类中如果出现和父类同名的方法，则会进行覆盖，调用的会后调用的是子类的方法；
```js
class Teacher extends Person {
    type: string
    constructor(type: string) {
        super('12')
        this.type = type
    }
}
```
### 访问修饰符
public ：公共，默认
private：私有的，只能在当前类中进行访问
protected：受保护的，这能在当前类或者子类中进行访问
readonly修饰符
可以使用readonly关键字将属性设置为只读，只读属性必须在声明时活构造函数里面被初始化

### 参数属性
可以方便的让我们在一个地方定义并初始化一个成员

```js
class Anmial {

    constructor(private name:string){}
    
}
```
### 类成员的存取器
在set的时候可以根据需求判断是否执行操作；

```js
class People{
    private _name:string = "";
    get name():string{
        return this._name
    }
    set name(value:string){
        this._name = value;
    }
}
```
## TypeScript的接口

### 基本使用
接口可以理解为一个约定或者一种规范

```js
interface AjaxOptins{
    url:string
    type:string
    data:object
    success(data:object):void;
}

function ajax(options:AjaxOptins){

}

ajax({
    url:"http",
    type:'post',
    data:{},
    success(data:object){

    }
})
```
### 接口的可选属性以及只读属性
可选属性：?

```js
interface AjaxOptins{
    url:string
    //type加了可选属性，代表，type不是必须的值，是可选的
    type?:string
    data:object
    success(data:object):void;
}

function ajax(options:AjaxOptins){

}

ajax({
    url:"http",
    data:{},
    success(data:object){

    }
})
```
只读属性

```js
interface Point{
    readonly x:number
    y:number
}

let poi:Point = {
    x:10,
    y:10,
}
//x为只读属性，重新赋值就会报错
poi.x =100;
```

### 接口的额外属性检查
可以在指定范围以外设置其他属性



```js
interface Point{
    readonly x:number
    y:number,
    //设置了额外的属性检查才可以为下面的增加z属性
    [propName:string]:any,
}

let poi:Point = {
    x:10,
    y:10,
    z:19,
}
```

### 函数类型接口

```js
interface SumInterface{
    (a:number,b:number):number
}

let sum:SumInterface = function(a:number,b:number){
    return a+b
}
```
### 类类型
```js
interface ClockInterFace{
    currentTime:Date,
}

class Clock implements ClockInterFace{
    currentTime:Date = new Date()
    constructor(h:number,m:number){

    }
}
```
### 接口继承接口
```js
interface TwoPointP {
    x: number,
    y: number,
}

interface ThreePointP extends TwoPointP {
    z: number
}

//必须指定x,y,z三项内容才可以
let poi2: ThreePointP = {
    z: 100,
    x: 100,
    y: 100
}
```
### 接口继承类
当接口继承一个类类型，他会继承类的成员但是不包括其实现。就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。接口同样会继承到类的private和protected成员。这意味着当你创建了一个接口继承了一个拥有私有或者被保护的成员的类时，这个接口类型只能被这个类或其子类所实现implement。

当你有一个庞大的继承结构这很有用，但要指出的是你的代码只在子类拥有特定属性时起作用，除了继承自基类，子类之间不必相关联。


```js
class Animal {
    name:string =""
    eat(){}
}

interface AnimalInterFace extends Animal{

}

class Bird implements AnimalInterFace {
    name:string = "12"
    eat(){}
}
```