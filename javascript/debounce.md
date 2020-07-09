## 1.函数防抖
说函数防抖，debounce。其概念其实是从机械开关和继电器的“去弹跳”（debounce）衍生 出来的，基本思路就是把多个信号合并为一个信号。

单反也有相似的概念，在拍照的时候手如果拿不稳晃的时候拍照一般手机是拍不出好照片的，因此智能手机是在你按一下时连续拍许多张， 能过合成手段，生成一张。翻译成JS就是，事件内的N个动作会变忽略，只有事件后由程序触发的动作只是有效。

```js
// 函数防抖
function debounce(func, delay) {
    var timeout;
    return function (e) {
        console.log("清除", timeout, e.target.value)
        clearTimeout(timeout);
        // console.log(arguments)
        var context = this, args = arguments
        console.log("新的", timeout, e.target.value)
        timeout = setTimeout(function () {
            console.log("----")
            func.apply(context, args);
        }, delay)
    };
};

var validate = debounce(function (e) {
    console.log("change", e.target.value, new Date - 0)
}, 1000);

// 绑定监听
document.querySelector("input").addEventListener('input', validate);
```
实现思路如下，将目标方法（动作）包装在setTimeout里面，然后这个方法是一个事件的回调函数，如果这个回调一直执行，那么这些动作就一直不执行。为什么不执行呢，我们搞了一个clearTimeout，这样setTimeout里的方法就不会执行！ 为什么要clearTimeout呢，我们就需要将事件内的连续动作删掉嘛！待到用户不触发这事件了。那么setTimeout就自然会执行这个方法。

那么这个方法用在什么地方呢，就是用于input输入框架的格式验证，假如只是验证都是字母也罢了，太简单了，不怎么耗性能，如果是验证是否身份证，这性能消耗大，你可以隔170ms才验证一次。这时就需要这个东西。或者你这个是自动完全，需要将已有的输入数据往后端拉一个列表，频繁的交互，后端肯定耗不起，这时也需要这个，如隔350ms。

## 2.函数节流
节流的概念可以想象一下水坝，你建了水坝在河道中，不能让水流动不了，你只能让水流慢些。换言之，你不能让用户的方法都不执行。如果这样干，就是debounce了。为了让用户的方法在某个时间段内只执行一次，我们需要保存上次执行的时间点与定时器。
函数节流会用在比input, keyup更频繁触发的事件中，如resize, touchmove, mousemove, scroll。throttle 会强制函数以固定的速率执行。因此这个方法比较适合应用于动画相关的场景。
```js
// 函数节流
function throttle(fn, threshhold) {
    var timeout
    var start = new Date;
    var threshhold = threshhold || 160
    return function () {
        var context = this, args = arguments, curr = new Date() - 0
        clearTimeout(timeout)//总是干掉事件回调
        if (curr - start >= threshhold) {
            console.log("now", curr, curr - start)//注意这里相减的结果，都差不多是160左右
            fn.apply(context, args) //只执行一部分方法，这些方法是在某个时间段内执行一次
            start = curr
        } else {
            //让方法在脱离事件后也能执行一次
            timeout = setTimeout(function () {
                fn.apply(context, args)
            }, threshhold);
        }
    }
}
var mousemove = throttle(function (e) {
    console.log(e.pageX, e.pageY)
},1000);

// 绑定监听
document.querySelector("#panel").addEventListener('mousemove', mousemove);

function test (a,b,c){
    console.log(arguments)
}
test(4,2,3)
```
## 区别：
函数防抖可以理解为在规定时间内如果屏蔽掉所有的行为操作，最后执行一次；
函数节流可以理解为你触发操作后，隔一段时间执行一次，减少执行的频率，防止页面假死；

## 无损耗防抖
无论节流还是防抖都会对用户的操作有所损耗，节流，时间内不执行，防抖，只进行一次，目前有这样的一个需求，在不损耗用户操作的情况下，如何处理？

保证每次用户的操作都会执行，或马上执行，或延迟执行
```js
// 函数防抖
function throttle(fn, threshhold) {
    var timeId
    var start = new Date;
    var threshhold = threshhold || 160
    var queue = 0; //缓存触发次数
    return function() {
        var context = this,
            args = arguments,
            curr = new Date() - 0
        timeId && clearInterval(timeId) //清理之前注册的计时器
        console.count();
        if (curr - start >= threshhold) {
            fn.apply(context, args)
            start = curr
        } else {
            queue++ //在没有调用函数的情况下缓存次数
            console.log(queue)
            timeId = setInterval(() => { //启用计时器
                fn.apply(context, args)
                queue--
                console.log('计时器' + queue)
                if (queue <= 0) { //递减为0的时候清理计时器
                    timeId && clearInterval(timeId)
                }
            }, threshhold);
        }
    }
}
var count = 0;
var mousemove = throttle(function(e) {
    count++
    console.log(count)
        // console.count()
}, 500);

// 绑定监听
document.addEventListener('scroll', mousemove);
```