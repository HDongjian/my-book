## Class类
可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已

 基本使用
```js
class Person {
    constructor(age, name) {
        this.age = age;
        this.name = name;
    }
}

console.log(new Person(12, '张三'))
```
### 构造器–constructor

构造器：每一个类中都有一个构造器，如果我们程序员没有手动指定构造器，那么可以认为类内部有一个隐形的，看不见的空构造器类似于constructor(){}

作用：就是每当new这个类的时候，必然会优先执行构造器中的代码

### 静态方法&静态属性
类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用

- 构造函数
```js
function PersonC(){}

PersonC.a = 1;
PersonC.b = function(){
    
}
```
- Class类

```js
class Person {
    constructor(age, name) {
        this.age = age;
        this.name = name;
    }
    static a = 12
    static b(){
        
    }
}
```
静态方法或者静态属性无法通过实例之后的对象来调用，因为这是挂载到类或者构造函数上面的，只能通过Person.a或者PersonC.a来获取或者调用

#### 实例方法&实例属性
通过new出来的实例，访问到的属性或者方法，叫做实例方法或者实例对象
```js
//构造函数
function PersonC(){}

PersonC.prototype.c = function(){

}

//class类
class Person {
    constructor(age, name) {
        this.age = age;
        this.name = name;
    }
    c(){

    }
}
```
### 继承
Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

#### 基本使用
```js
class Person {
    constructor(age, name) {
        this.age = age;
        this.name = name;
    }
}

class Chiness extends Person{

}
console.log(new Chiness(12,'李四'))
```
#### 关于super方法
在以上代码中如果我们在Chiness中值只调用配置constructor函数，就会报错

```js
class Chiness extends Person{
    constructor(){
    }
}

// missing super() call in constructor

class Chiness extends Person{
    constructor(){
        super()
    }
}

console.log(new Chiness(12,'李四'))

//Chiness {age: undefined, name: undefined}
```
但是如果我们调用了，不会报错，但是实例对象中的age和name属性的值为undefined

就会提醒我们调用super方法

为什么必须调用super?

答：如果一个子类，通过extends关键字继承了父类，那么，子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。

根据以上说法，如果想在构造器中使用this也必须是在super调用之后

super方法是什么?

答:super是一个函数，而且它是父类的构造器，子类中的super就是其中父类中，constructor构造器的一个引用

为什么调用之后实例对象中的age和name属性的值为undefined?

答:super是一个函数，而且它是父类的构造器,所以如果只调用，不传递参数，所以实例对象中的age和name属性的值为undefined

正确用法
```js
class Chiness extends Person{
    constructor(age,name){
        super(age,name)
    }
}
console.log(new Chiness(12,'李四'))
```
#### 关于子类独有属性
```js
class Chiness extends Person{
    constructor(age,name,id){
        super(age,name)
        this.id = id;
    }
}

console.log(new Chiness(12,'李四',12))
```
