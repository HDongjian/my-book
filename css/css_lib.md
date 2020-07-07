
## 多行最后一行 ···

```CSS
.answer-body {
  font-size: 16px;
  line-height: 30px;
  text-align: left;
  height: 60px;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

```

## 单行溢出 ...

```css
width:300px;    
overflow: hidden;    
text-overflow:ellipsis;    
whitewhite-space: nowrap;

```

## 移动端页面切换动画

```css
.vux-pop-in-leave-active {
  will-change: transform;
  transition: transform 500ms;
  height: 100% !important;
  top: 0 !important;
  position: absolute !important;
  backface-visibility: hidden;
  perspective: 1000;
  transform: translate(-100%, 0);
}
.vux-pop-in-enter {
  transform: translate(100%, 0);
}
.vux-pop-in-enter-active {
  will-change: transform;
  transition: transform 500ms;
  height: 100% !important;
  top: 0 !important;
  position: absolute !important;
  backface-visibility: hidden;
  perspective: 1000;
}
.vux-pop-out-leave-active {
  will-change: transform;
  transition: transform 500ms;
  height: 100% !important;
  top: 0 !important;
  position: absolute !important;
  backface-visibility: hidden;
  perspective: 1000;
  transform: translate(100%, 0);
}
.vux-pop-out-enter {
  transform: translate(-100%, 0);
}
.vux-pop-out-enter-active {
  will-change: transform;
  transition: transform 500ms;
  height: 100% !important;
  top: 0 !important;
  position: absolute !important;
  backface-visibility: hidden;
  perspective: 1000;
}
@-webkit-keyframes rotating {
  from {
    -webkit-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  to {
    -webkit-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes rotating {
  from {
    -ms-transform: rotate(0deg);
    -moz-transform: rotate(0deg);
    -webkit-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  to {
    -ms-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -webkit-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
.rotating {
  -webkit-animation: rotating 2s linear infinite;
  -moz-animation: rotating 2s linear infinite;
  -ms-animation: rotating 2s linear infinite;
  -o-animation: rotating 2s linear infinite;
  animation: rotating 2s linear infinite;
}
```
```less
.vue-pop-base{
  will-change: transform;
  transition: transform 500ms;
  height: 100% !important;
  top: 0 !important;
  position: absolute !important;
  backface-visibility: hidden;
  perspective: 1000;
}

.pop-transition(@className,@leave,@enter){
  .vux-pop-@{className}-leave-active {
    .vue-pop-base;
    transform: @leave;
  }
  
  .vux-pop-@{className}-enter {
    transform: @enter;
  }
  
  .vux-pop-@{className}-enter-active {
    .vue-pop-base;
  }
}

.pop-transition(in,translate(-100%, 0),translate(100%, 0));
.pop-transition(out,translate(100%, 0),translate(-100%, 0));


@-webkit-keyframes rotating {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg)
  }
  to {
    -webkit-transform: rotate(1turn);
    transform: rotate(1turn)
  }
}

@keyframes rotating {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg)
  }
  to {
    -webkit-transform: rotate(1turn);
    transform: rotate(1turn)
  }
}

.rotating {
  -webkit-animation: rotating 2s linear infinite;
  animation: rotating 2s linear infinite
}
```

## 三角形

```css
div {
  width: 0;
  height: 0;
  border-width: 0 40px 40px;
  border-style: solid;
  border-color: transparent transparent red;
}
```

## 边框三角形

```css
<div id="blue"><div>

#blue {
  position:relative;
  width: 0;
  height: 0;
  border-width: 0 40px 40px;
  border-style: solid;
  border-color: transparent transparent blue;
}
#blue:after {
  content: "";
  position: absolute;
  top: 2px;
  left: -38px;
  border-width: 0 38px 38px;
  border-style: solid;
  border-color: transparent transparent #fff;
}
```

## 图片旋转

```html
<div class="rotate-box">
  <img class="rotate-img" src="../../static/images/small.png" alt="">
</div>
```
```css
.rotate-img {
  transform: rotate(360deg);
  -webkit-transform: rotate(360deg);
  animation: rotation 5s linear infinite;
  -moz-animation: rotation 5s linear infinite;
  -webkit-animation: rotation 5s linear infinite;
  -o-animation: rotation 5s linear infinite;
}
@-webkit-keyframes rotation{
  from {-webkit-transform: rotate(0deg);}
  to {-webkit-transform: rotate(360deg);}
}

```