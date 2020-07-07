
# 简介
运算符    描述
&&    and
ll    or
!    not
这个傻子都知道

# 关于true和false
在javascript中，数据类型可以分为“真值”和“假值”。真值转换为bool时值为true；假值转换为bool时值为false
```js
console.log(Boolean("")) //false
console.log(Boolean(0)) //false
console.log(Boolean(1)) //true
console.log(Boolean(null)) //false
console.log(Boolean(undefined)) //false
console.log(Boolean([])) //true
console.log(Boolean({})) //true
console.log(Boolean(function() {})) //true
```
# 关于短路
如果&&左侧表达式的值为真值，则返回右侧表达式的值；否则返回左侧表达式的值。
如果||左侧表达式的值为真值，则返回左侧表达式的值；否则返回右侧表达式的值。
```js
console.log('' && '1')
console.log(2 && 0)
console.log('' || '1')
console.log(2 || 0)
```
# 关于应用
## 场景1
假设对成长速度显示规定如下：

成长速度为5显示1个箭头；
成长速度为10显示2个箭头；
成长速度为12显示3个箭头；
成长速度为15显示4个箭头；
其他都显示都显示0各箭头。
使用分支结构：if、switch
```js
var add_level = 0;
if(add_step == 5){
  add_level = 1;
}
else if(add_step == 10){
  add_level = 2;
} 
else if(add_step == 12){
  add_level = 3;
}
else if(add_step == 15){
  add_level = 4;
}
else {
  add_level = 0;
}

var add_level = 0;
switch(add_step){
  case 5 : add_level = 1;
  break;
  case 10 : add_level = 2;
  break;
  case 12 : add_level = 3;
  break;
  case 15 : add_level = 4;
  break;
  default : add_level = 0;
  break;
}
```
使用运算符
```js
add_level = (add_step==5 && 1) || (add_step==10 && 2) || (add_step==12 && 3) || (add_step==15 && 4) || 0;
// 或者
var add_level={'5':1,'10':2,'12':3,'15':4}[add_step] || 0;
```
## 场景2
成长速度为>12显示4个箭头；
成长速度为>10显示3个箭头；
成长速度为>5显示2个箭头；
成长速度为>0显示1个箭头；
成长速度为<=0显示0个箭头。
那么用switch实现起来也很麻烦了，而且使用对象也没办法了，现在就到了展现真实实力的机会了
```js
var add_level = (add_step>12 && 4) || (add_step>10 && 3) || (add_step>5 && 2) || (add_step>0 && 1) || 0;
```
## 场景3
短路的应用
```js
a=a||"defaultValue";
if(!a){
  a="defaultValue";
}
if(a==null||a==""||a==undefined){
  a="defaultValue";
}

//防止回调函数未传报错
callback&&callback()
```