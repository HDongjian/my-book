# 简单Less用法


## 路径引入
```less
// Variables
@images: "../img";

// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}


// Variables
@themes: "../../src/themes";
// Usage
@import "@{themes}/tidal-wave.less";
```
## 属性代替

```less
@property: color;

.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```
## 延伸选择器
### 初步用法
```less
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
输出

nav ul {
  background: blue;
}
.inline,
nav ul {
  color: red;
}
```
### 对比混入
示例 - 使用mixin：
```less
.my-inline-block() {
    display: inline-block;
  font-size: 0;
}
.thing1 {
  .my-inline-block;
}
.thing2 {
  .my-inline-block;
}
输出
```less
.thing1 {
  display: inline-block;
  font-size: 0;
}
.thing2 {
  display: inline-block;
  font-size: 0;
}
.my-inline-block {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  &:extend(.my-inline-block);
}
.thing2 {
  &:extend(.my-inline-block);
}
输出
```less
.my-inline-block,
.thing1,
.thing2 {
  display: inline-block;
  font-size: 0;
}
```
## Mixins中的选择器
```less
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```
输出
```less
button:hover {
  border: 1px solid red;
}
```
## 循环
### 属性循环
```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}
```
输出：
```less
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```
### 类名循环
```less
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
```
输出：
```less
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```
## 合并
```less
.mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```
输出
```less
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```
## 父选择器
### 属性
```less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
结果是：
```less
a {
  color: blue;
}

a:hover {
  color: green;
}
```
### 类名
```less
.button {
  &-ok {
    background-image: url("ok.png");
  }
  &-cancel {
    background-image: url("cancel.png");
  }

  &-custom {
    background-image: url("custom.png");
  }
}
```
输出：
```less
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```