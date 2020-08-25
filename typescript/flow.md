
## 安装步骤

```shell
npm init -y 
npm i flow-bin -D
```
## 使用步骤

### 声明flow
需要给文件添加@flow 标记，否则flow不会对文件进行类型检测
```js
//@flow
```
### 书写方式
通过注释的方式进行添加（不会改写js）
```js
var a /*:number */ = 12;
a = "111";
console.log(a)
```
通过直接给数据添加类型，改写js代码，如果需要正常运行，需要进行babel进行转码
```js
var a: number = 12;
a = 1;
console.log(a);
```
### 使用flow进行检查

在package.json中添加命令
```json
"flow": "flow"
```
创建flow配置文件.flowconfig
```shell
npm run flow init
```

## 使用babel对flow进行转码

如果给数据添加类型声明通过第二种方式，直接修改js代码，那么代码是不能正常运行的；

我们需要通过babel对代码进行转码之后才能正常进行

## 安装bebel以及persets
```shell
npm i babel-cli babel-preset-flow -D
```
新建 .babelrc文件并添加配置

```json
{
    "presets": [
        "flow"
    ]
}
```
配置package.json 添加build命令调用babel

```shell
"build":"babel ./src -d ./dist"
```
执行build命令进行转换

## Flow的数据类型

类型说明
类型	说明
number	数字、NaN、Infinity都是number类型的数据
string	字符串
null	只有null是null类型的
void	undefined在flow中类型就是void
Array	数组类型，定义的时候需要使用 Array这种形式，T为指定的类型，说的是特定类型的数据组成的数组
Object	对象类型，由于对象比较自由，可以规定对象类型的时候有多中写法
any	表示任意类型，这个尽量少用，但是有时有很有用
Functions	函数类型
Maybe	Maybe类型允许我们声明一个包含null和undefined两个潜在类型的值
或操作	或操作可以设置一个变量为多种可能的类型
类型判断	flow会常识自行腿短某个数据的类型

## 部分案例

```js
//@flow


//1.数组
let a: Array < number > = [1, 2, 3];


//2.函数

function test(a: number, b: number): number {
    return a + b;
};

test(1, 2);


//3.maybe

function test2(a: ? number) {
    a = a || 0;
    console.log(a)
}

test2()
test2(1)


//3.或 |

let b: number | string = 1;

b = 1;
b = '1'


//4.Object

function greet(obj: { sayHello: () => void, name: string }) {
    obj.sayHello();
}
```