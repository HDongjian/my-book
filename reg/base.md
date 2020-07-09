
## 关于正则表达式
### 简介
正则表达式(Regular Expression)是一种文本模式，包括普通字符（例如，a 到 z 之间的字母）和特殊字符（称为”元字符”）。

正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串。
```js
var str = "abc123def";
var patt1 = /[0-9]+/;
document.write(str.match(patt1));
```
### 为什么要用正则表达式
典型的搜索和替换操作要求您提供与预期的搜索结果匹配的确切文本。虽然这种技术对于对静态文本执行简单搜索和替换任务可能已经足够了，但它缺乏灵活性，若采用这种方法搜索动态文本，即使不是不可能，至少也会变得很困难。

通过使用正则表达式，可以：

- 测试字符串内的模式。
例如，可以测试输入字符串，以查看字符串内是否出现电话号码模式或信用卡号码模式。这称为数据验证。

- 替换文本。
可以使用正则表达式来识别文档中的特定文本，完全删除该文本或者用其他文本替换它。

- 基于模式匹配从字符串中提取子字符串。
可以查找文档内或输入域内特定的文本。

## 元字符
### 常用元字符汇总表
符号    描述
.    表示出了\n以外任意的一个单个的字符串；
\d    匹配一个数字字符。等价于 [0-9]
\D    匹配一个非数字字符。等价于 [^0-9]。
\s    匹配任何空白字符，包括空格、制表符、换页符等等
\S    匹配任何非空白字符
\w    匹配字母、数字、下划线。等价于’[A-Za-z0-9_]’
\W    匹配非字母、数字、下划线。等价于 ‘[^A-Za-z0-9_]’
### 特殊的元字符 –限定符
符号    描述
*    匹配前面的子表达式零次或者多次。
+    匹配前面的子表达式一次或者多次。
？    匹配前面的子表达式零次或者一次。
{n}    匹配确定的n次。
{n,}    至少匹配n次。
{n,m}    最少匹配n次，且最多能匹配m次。
### 其他特殊符号
符号    描述
[]    查找方括号之间的任何字符,可以理解为一个范围，或者表示一个单独字符
[a-z]    表示小写字母
[A-Z]    表示大写字母
[0-9]    表示数字
[a-zA-Z0-9]    表示所有数字和字母
[\u4e00-\u9fa5]    校验中文
\    将下一个字符标记为一个特殊字符、或一个原义字符：转义
^    表示以什么开始或者取反
$    表示以什么结束
竖线    指明两项之间的一个选择（优先级最低）
()    小括号：提升优先级别的；作用：分组 从最左边算起
### 案例
```js
console.log(/./.test('除了换行以外的任意字符'))
console.log(/.*/.test(''))
console.log(/.+/.test(''))
console.log(/b|(ara)/.test('abra'))
console.log(/^b|(ara)$/.test('abra'))
console.log(/[a-z]{2,3}/.test('ar'))
console.log(/\w{2}/.test('abc23'))
```
## RegExp 对象
### 简介
正则表达式是描述字符模式的对象。

正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。

### 使用
### 关于参数
pattern（模式） 描述了表达式的模式
modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配
修饰符    描述
i    执行对大小写不敏感的匹配。
g    执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
m    执行多行匹配。
### 构造函数
```js
var patt=new RegExp(pattern,modifiers);
```
### 使用字面量
```js
var patt=/pattern/modifiers;
```
### 对象方法
#### test()方法
test() 方法用于检测一个字符串是否匹配某个模式.
如果字符串中有匹配的值返回 true ，否则返回 false。

//关于严格模式
```js
var reg = new RegExp(/^\d{5}$/);
console.log(reg.test('12345')) //true
console.log(reg.test('123456')) //false
```
#### exec()方法
exec() 方法用于检索字符串中的正则表达式的匹配。

如果字符串中有匹配的值返回该匹配值，否则返回 null。
```js
console.log(/\d/.exec('adf12'))
```
#### toString()
返回正则表达式的字符串值。
```js
var res = /\d/.toString();
console.log(res) // /\d/
```
### 支持正则的String方法
#### replace()
replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
```js
var str = "Mr Blue has a blue house and a blue car";
console.log(str.replace(/blue/, "red"))
console.log(str.replace(/blue/g, "red"))
```
#### search()
用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
```js
console.log('1212adfvafed'.search(/\d/)) //0
```
#### match()
可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。

```js
var str = "The rain in SPAIN stays mainly in the plain";
var n = str.match(/ain/gi);
console.log(n)
```
#### split()
用于把一个字符串分割成字符串数组。
```js
var str = "The rain in SPAIN stays mainly in the plain";
var n = str.match(/ain/gi);
```
