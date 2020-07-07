
变量提升是JS在非严格模式下，JS解析的时候出现的一种情况

JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。
JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。
# 变量提升
```js
console.log(first) //undefined
var first = 124;
```
/*
正常var变量声明之后会被提升到最顶端，但是是声明提前，赋值不提前，所以为undefined
*/
# 函数提升
```js
second(); //123
function second() {
  console.log(123)
}
/*
征程通过function声明之后函数会被提升到最顶端，可以直接在声明之前调用
*/
```
# 函数提升
```js
// third(); //14-变量提升.html:28 Uncaught TypeError: third is not a function
var third = function () {
console.log('第三种')
}
/*
页面报错，因为var声明变量，声明提前，赋值不提前，所以trird为undefined，所以报错；
*/
```
# 函数和变量提升对比
```js
console.log(fifth) //ƒ fifth(){
// console.log(123)
// }
var fifth = 123;
function fifth() {
console.log(123)
}
/*
函数和变量同时声明，函数覆盖变量，在进行变量提升；
*/
console.log(six) //ƒ six(){
// console.log(123)
// }
function six() {
console.log(123)
}
var six = 123;
/*
函数和变量同时声明，函数覆盖变量，在进行变量提升；
*/
```