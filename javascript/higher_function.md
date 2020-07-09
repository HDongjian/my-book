
# 概念
作为参数的函数
作为返回值的函数 都可以称为高阶函数
# 回调函数
回调函数就是一个参数，将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，再执行传进去的这个函数。这个过程就叫做回调。

# 函数柯里化Currying
把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术
```js
function currying(fn) {
    var slice = Array.prototype.slice,
    __args = slice.call(arguments, 1);
    return function () {
        var __inargs = slice.call(arguments);
        return fn.apply(null, __args.concat(__inargs));
    };
}
```
柯里化特性决定了它这应用场景。提前把易变因素，传参固定下来，生成一个更明确的应用函数。最典型的代表应用，是bind函数用以固定this这个易变对象。
```js
Function.prototype.bind = function(context) {
    var _this = this,
    _args = Array.prototype.slice.call(arguments, 1);
    return function() {
        return _this.apply(context, _args.concat(Array.prototype.slice.call(arguments)))
    }
}
# 案例
function currying(fn) {
    var slice = Array.prototype.slice,
        __args = slice.call(arguments, 1);
    console.log(__args)
    return function () {
        var __inargs = slice.call(arguments);
        console.log(__inargs)
        console.log(__args.concat(__inargs))
        return fn.apply(null, __args.concat(__inargs));
    };
}
function square(i) {
    console.log(i)
    return i * i;
}

function dubble(i) {
    return i *= 2;
}

function map(handeler, list) {
    return list.map(handeler);
}

var mapSQ = currying(map, square);
console.log(mapSQ([1, 2, 3, 4, 5]));
```