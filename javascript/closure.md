# 闭包的概念：
函数的闭包形成闭包；封闭的区域（可以调用其他作用域变量的函数）；
闭包的组成：内层函数本身；内层函数所处的作用域；


# 闭包作用：
## 形成隔离的封闭空间；
//函数里面的num不会影响外面num的值；
var num = 100;
(function(){
    var num = 1000;
})()
console.log(num)
## 延长变量的声明周期；
## 匿名自制行环境–沙箱
插件常用的内部变量外放的操作，将window和document作为实参放入沙箱之内，防止内部变量对于外部的影响
减少作用域链；
```js
(function(window, doc){
    if (typeof exports !== 'undefined') exports.touchScroll = iScroll;
    else window.touchScroll = iScroll;
})(window, document);
```
## 缓存变量
```js
var arr = [];
        for(var i=0;i<10;i++){
            arr[i] = (function (n){
                return function(){
                    console.log(n)
                }
            })(i)
        };
        arr[3](); //结果是3；
```
## 实现类和继承
```js
function Person(){
        var name = "default";
        return {
           getName : function(){
               return name;
           },
           setName : function(newName){
               name = newName;
           }
        }
    };


    var p = new Person();
    p.setName("Tom");
    alert(p.getName());


    var Jack = function(){};
    //继承自Person
    Jack.prototype = new Person();
    //添加私有方法
    Jack.prototype.Say = function(){
        alert("Hello,my name is Jack");
    };
    var j = new Jack();
    j.setName("Jack");
    j.Say();
    alert(j.getName());
```
## 大量封装函数
```js
var person = function(){
    // var num = '0';
    return {
        sum : function (num) {
            var sum = 0;
            for(var i=0;i<=num;i++){
                sum+=i;
            }
            return sum;
        },
        mul : function (num) {
            var mul = 1;
            for(var i = 1;i<=num;i++){
                mul*=i;
            }
            return mul;
        }
    }
}();
```
# 案例
```js
var arr=[];
    for(var i=0;i<3;i++){
        arr[i]=(function(j){return function(){console.log(j)}})(i)
    }
    arr[0]();
    arr[1]();
    arr[2]();
//生成随机数
    function getNum(){
        var num=parseInt(Math.random()*10+1);
        return function(){
            return num;
        }
    };
```


# 局部作用域和垃圾回收机制
```js
function a() {
    var b = 12;
}
// a();
function c() {
    d = 12;
}
c();
// delete window.d;
console.dir(window.d)
```
js的垃圾回收机制(GC:Garbage Collecation)会定期清除无用的变量，释放内存空间，首先释放的就是生命周期结束的变量，全局变量的生命周期直至浏览器卸载页面才会结束。局部变量只在函数的执行过程中存在，所以，在函数执行完之后打印b是会报错的
此处说明一下函数c内部的情况，声明变量不加var，为隐式声明，可以理解为window.d = 12；注意虽然是叫隐式声明，但是b此时并不是一个变量，而是window的一个属性，下面会专门找时间把变量和属性的区别描述清楚；

# 全局对象的属性和函数声明（这个属于插一曲）
```js
var e = 123;
window.f = 1213
console.log(f)
delete window.e
delete window.f
console.dir(e)
console.log(window);
```
全局函数声明虽然也是挂载到window下的一个属性，但是函数声明和属性的区别是，函数声明拥有不可删除性，通过delect方法无法删除

# 局部作用域和闭包
```js
//闭包案例
function z() {
    var num = 0;
    return function() {
        return num++
    }
}
var y = z();
console.dir(z)
console.log(y())
console.log(y())
```
调用一次返回值+1；
正常函数调用完之后，局部作用域内声明的变量会被清除，但是如果在局部作用域函数有函数引用变量，形成闭包环境，该变量则不会清除，就会挂载到该局部作用域；所以函数调用一次就会增加一次形成一个缓存的作用，这也是为什么闭包会容易造成内存溢出的原因；
//即使是局部作用域的函数未清除，全局作用域仍然无法获取该变量；所以这也是自执行函数IFEE能够形成自己独立区域的原因，防止变量污染；

块级作用域
```js
//所谓的块级作用域就是{}
{
    var aa = 1
}
console.log(aa) //1;

//使用var声明变量没有块级作用域，常见的还有for循环中的变量i值
{
    let bb = 1;
    console.log(bb) //1;
}
console.log(bb) //: bb is not defined

// 使用es6的let和const可以生成一个块级作用域，所以之前的自调用函数，可以使用{}代替
```