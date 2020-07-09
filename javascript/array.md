# Array

### 数组去重
```js
Array.from(new Set([1,2,3,3,4,4])) //[1,2,3,4]
[...new Set([1,2,3,3,4,4])] //[1,2,3,4]
```
### 是否包含某值
```js
[1,2,3].includes(4) //false
[1,2,3].indexOf(4) //-1 如果存在换回索引
[1, 2, 3].find((item)=>item===3)) //3 如果数组中无值返回undefined
[1, 2, 3].findIndex((item)=>item===3)) //2 如果数组中无值返回-1
```
### 类数组转换
```js
Array.prototype.slice.call(arguments) //arguments是类数组(伪数组)
Array.prototype.slice.apply(arguments)
Array.from(arguments)
[...arguments]
```
### 提取对象key
```js
Object.keys({name:'张三',age:14}) //['name','age']
```
### 提取对象value
```js
Object.values({name:'张三',age:14}) //['张三',14]
```
### 提取对象key和value
```js
Object.entries({name:'张三',age:14}) //[[name,'张三'],[age,14]]
```
### 将提取的key和value放回去
```js
Object.fromEntries([name,'张三'],[age,14]) //ES10的api,Chrome不支持 , firebox输出{name:'张三',age:14}
```
### 每一项设置值
```js
[1,2,3].fill(false) //[false,false,false] 
[1,2,3].map(() => 0)
```
### 每一项是否满足
```js
[1,2,3].every(item=>{return item>2}) //false
```
### 有一项满足
```js
[1,2,3].some(item=>{return item>2}) //true
```
### 过滤数组
```js
[1,2,3].filter(item=>{return item>2}) //[3]
```
### 按照索引删除元素
```js
[1,2,3].splice(index,1) //[1,3]