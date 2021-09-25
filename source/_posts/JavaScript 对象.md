---
title: JavaScript 对象
tags:
- 前端
- JavaScript
categories: 编程
---
## 概念

对象是JavaScript的一个基本数据类型，是一种复合值，它将很多值（原始值或者其他对象）聚合在一起，可通过名字访问这些值。即属性的无序集合。

## 创建方法

1 对象字面量/对象直接量

（字面量就是指这个量本身，比如字面量3。也就是指3. 再比如 string类型的字面量"ABC", 这个"ABC" 通过字来描述。 所以就是字面量，虽然很难下定义。 你就理解成一眼就能知道的量。）

```JavaScript
var obj ={
    name:"xiaoming",
    age: 12,
    eat : function(){
        console.log("food");
    },
}
```

2 构造函数

2.1 自定义构造函数

类似于创建了一个工厂，可以批量生产对象，比如：

```js
function Car(color){//相当于一个工厂模板
    this.color = color;
    this.name="BMW";
    this.height = "1400";
    this.weight = 1000;
    this.run = function(){
	this.health --;
    }
}

var car1 = new Car("red");
var car2 = new Car("blue");
```

构造函数内部原理（有new)//用来区分构造函数与函数

1） 在函数体最前面隐式的加上 this={}

2） 执行 this.xxx=xxx;

3） 隐式的返回this

2.2 系统自带的构造函数

比如 new Object()  //不常用

new String() new Number() 等等，相当于系统把“工厂模板写好了”

3 Object.create(原型)

```js
Person.prototype.name="xiao";
function Person(){
    
}

var person=Object.create(Person.prototype)
```



## 包装对象

js里有原始值和对象。

对象可以随便的增加属性和方法。或删除属性和方法 {用delelte(对象.属性) }。

原始值应该是不能调用属性和方法的，但是js“可以调用”，这就是因为js在后台创建了包装对象。

```js
var s="hello";
s.index=1;
var t=s.index;
console.log(t)//undefined
```

这里给s设置了index属性，但是却无法访问，是因为**包装对象在后台创建完成并返回值后会立即销毁。**

细节如下：

```js
var s="hello";
s.index=1;//此时会创建一个new String("hello"),
//并增加一个index属性=1，之后销毁
var t=s.index;//该对象被销毁了，所以并不能访问
console.log(t)//undefined
```

这就很好的解释了原始值调用属性的问题。比如：

```js
var str="123456";
//创建new String("123456"),得到length返回后，销毁
console.log(str.length);//6
```

## 原型

定义：原型是function对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。**原型也是对象。**

```js
function Person(){
}
//在函数"出生时"，Person.prototype(原型)就被定义好了
//Person.prototype是个对象，相当于Person生产出的对象的共同祖先
var person = new Person();
```

利用原型特点和概念，可以提取公用属性。

比如，Person.prototype.LastName="li" 那么。通过Person()函数构造出的函数都会有这个LastName属性。值为”li",当然，个体也可以更改LastName的值。

对象查看自己的构造函数 --> constructor（构造器）

```js
function Person(){
}
var person = new Person();
person.constructor//functoion Person(){}
```

constructor可以改变。

### 原型链 

对象查看原型：\_\_proto\_\_ （隐式属性）

\_\_proto\_\_  属性里面放的是原型 （\_\_proto\_\_  前后有两个`_`） 

例子：

```js
function Person(){
    //会有一个隐藏属性
    //this ={__proto__: Person.prototype}当new时创建。参考上面的构造函数原理
    //这里是一个钩子，当this里没有调用的属性是，会去__proto__指向的地址寻找
}
```

`__proto__` 的值可以修改,也就是说明自己可以更改自己的祖先。

```js
function Grand(){
    this.lastName="xiao";
}
var grand = new Grand();

function Father(){
}

var father =new Father();
father.__proto__=grand;
function Son(){
}

var son= new Son();
son.__proto__=father;
console.log(son.lastName);//xiao
```

这就构成了一个原型链，son没有lastName会按着原型链往上寻找，直到找到输出。

### 区分prototype 和 \_\_proto\_\_

prototype是**函数属性**，代表该构造函数所产生的对象的”爹“，也是函数创建对象的`__proto__` 所指

`__proto__` 是一个**对象的属性**，它指向该对象的原型

```
JS中每个对象都会有__proto__属性，默认为Object，例如：
var a={};//这里对象a的__proto__属性就是Object
```

原型链的终端是Object.prototype, 而Object有toString方法，所以大部分对象都能调用toString()方法。

obj.toString()会输出[Object Object],输出的格式是[object 对象的类型] 

## call/apply

 call/apply作用的是改变this指向

方法.call(this的指向，参数)

```js
function Person(name, age, sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
}

function Student(name, age, sex, tel, grade){
    Person.call(this,name,age,sex);
    this.tel=tel;
    this.grade=grade;
}

var student=New Student(...);
```

这样Student就包含了Person的属性

Person中的this指向了student

apply的作用域call一样。

不同的是，call 需要把实参按照形参个数传进去。

apply 需要传一个argument，（就是在参数两边加[])

## 继承 

传统方式： 原型链 过多的继承了没用的方法。

共享原型：

```js
Father.prototyper={
    
}

function Father(){
    
}

function Son(){

}

Son.prototype = Father.prototype
```

这样Father于Son就公用继承了一个原型。但是想要给Son单独增加原型属性，就会影响Father的属性，所以需要改进。

圣杯模式：找一个中间层

```js
Father.prototyper={
    
}

function Father(){
    
}

function Son(){

}

function F(){
    
}

F.prototype = Father.prototype
Son.ptototype=New F()
```

这样只给F增加属性，不会影响到Father了。

## 对象枚举

**for...in**

```js
var obj ={
    name:"dfasdf",
    sex:"male",
    weight:123,
    eat: function(){
        console.log("apple");
    }
}

for(var prop in obj){
    console.log(prop);
}
```

会把对象的属性名和方法名赋给prop

**obj.prop系统内部会转换成obj["prop"],寻找prop属性**

**所以用for...in输出对象，要用obj[prop]** 属性就是字符串

**hasOwnProperty()** 

可以用来判断是否是自己的属性（是自己的返回真，原型链上的返回假）

 **in**  

”属性“ in obj 用来判断该属性是否是obj的属性，（包括原型链上的属性）

**instranceof**

用来判断 A对象 是否是B函数构造出来的

A对象 instranceof B函数

看A对象的原型链 有没有B的原型

## this

函数预编译过程 this --> window

全局作用域里 this-->window

call//apply 改变函数运行时this的指向

obj.func(),fun()里面的this指向obj

## 类数组

对于一个普通的对象来说，有相应的length属性，（最好加上push) 那么虽然该对象并不是由Array构造函数所创建的，它依然呈现出数组的行为，在这种情况下，这些对象被称为“类数组对象”。

比如

```js
var obj={
    0:"a",
    1:"b",
    2:"c",
    length:3,
    push:Array.prototype.push
}
```

特点：

可以利用属性名模拟数组的特性

可以动态的增长length属性

如果强行让类数组调用push方法，则会根据length属性值的位置进行属性的扩充。

push的原理：

```js
Array.prototype.push=function(target){
    obj[obj.length]=target;
    obj.length++;
}
```

