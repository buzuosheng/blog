---
title: JS实现继承
date: 2020-10-04 23:00:42
tags:  面试
description:
categories: 面试
---

在面向对象编程中有一个很重要的特性，就是**继承**，通过继承可以减小大量冗余的代码。

JS也是可以面向对象编程的，在JS里也有多重继承方式。

## class继承

`class`是ES6增加的关键字，他的本质还是函数。

使用class继承非常简单。子类使用`extends`关键字表明继承于哪个类，并在子类中调用`super()`,这相当于使用`call()`改变this的指向。

class继承代码如下：

``` js
class Person {
  constructor(value){
    this.value = value
  }
    
  getValue() {
    console.log(this.value)
  }
}

class person {
  constructor(value){
    super(value)
  }
}

let p = new person()
p instanceof person	//true
p instanceof Person	//true
```

## 原型链继承

``` js
function Name() {
  this.name = 'name'
}

function Age() {
  this.age = 'age'
}
Age.prototype = new Name()	//Age的原型对象是Name的实例对象，将Age添加到原型链上
let age = new Age
console.log(age.name, age.age)	//name age
```

原型链继承就是使用的它的概念`对象._proto_ = 构造函数.prototype`

## 构造函数继承

``` js
function Father(age) {
  this.name = ['zhao', 'qian']
  this.age = age
}
function Son(age) {
  Father.call(this, age)
}
let son = new Son(12)
console.log(son.age)	// 12
console.log(son.name)	// ["zhao", "qian"]
son.name.push('zhou')	// 3
console.lgo(son.name)	// ["zhao", "qian", "zhou"]
```

## 组合继承

组合继承是**原型链继承+构造函数继承**，原型链继承的属性，构造函数继承方法。原型链继承会出现一个问题：**包含引用类型值的原型属性，会被所有实例共享**。构造函数继承的问题是：**没有原型，无法复用**。

组合继承代码如下：

``` js
function Father(age) {
  this.name = ['zhao', 'qian']
  this.age = age
}
Father.prototype.run = () => { return this.name + ' ' + this.age}

function Son(age) {
  Father.call(this, age)
}

Son.prototype = new Father()
let son = new Son(12)
console.log(son.run())	//zhao,qian 12
```

这种继承方式通过`Father.call(this)`继承父类的属性，通过`new Father()`继承父类的函数。优点在于**构造函数可以传参，不会与父类共享属性**，缺点是**在继承父类函数的时候调用了父类的构造函数**。

## 寄生组合继承

组合继承的缺点是在继承时调用了父类的构造函数。寄生组合继承解决了两次调用的问题。

``` js
function Parent (name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}
  
Parent.prototype.getName = () => { console.log(this.name) }
  
function Child (name, age) {
  Parent.call(this, name);
  this.age = age;
}
 
function prototype(Child,Parent){ //获得父类型原型上的方法
    let prototype = object(Parent.prototype) //创建父类型原型的一个副本，相当于prototype.__proto__ = Parent.prototype
    prototype.constructor = Child
    Child.prototype = prototype
}
 
prototype(Child,Parent) //必须执行这个函数，才能进行给子类型原型写方法，顺序调转的话子类型原型会被重写
Child.prototype.getName = () => { console.log(this.name) }
```

## 总结

- 原型链继承来自**父类原型对象的引用属性是所有子类共享的；创建子类实例时，无法向父类构造函数传参**。

- 构造函数继承解决了上述问题，但**无法实现函数的复用**，方法在构造函数中定义，每次创建子类实例都会创建一个新方法，占用内存。
- 组合继承解决了上述问题，**使用原型继承继承父类的属性`Parent.call(this)`，使用构造函数继承父类的方法`new Parent()`**。但子类调用了两次父类构造函数，生成了两个父类实例。
- 寄生组合继承解决了上述问题，但是使用复杂。
- class是ES6的语法，使用`extends`指定继承的父类。

