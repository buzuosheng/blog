---
title: JS中的原型和原型链
date: 2019-12-21 22:04:13
tags:
---

## 原型

JavaScript规定，每一个函数都有一个prototype对象属性，指向另一个对象。prototype对象属性的所有属性和方法都会被构造函数的实例继承。这意味着我们可以把那些公用的属性和方法，直接定义在prototype对象属性上。

prototype就是调用构造函数所创建的实例对象的原型（proto）。js在创建对象的时候，都有一个叫做proto的属性，用于指向它的函数对象的原型对象prototype。

prototype可以让所有的对象实例共享它包含的属性和方法。

## 原型链

每一个对象都可以有一个原型，这可原型还可以有它自己的原型，以此类推，就形成了原型链。

查找一个对象的属性或方法的时候，如果这个对象中没有这个属性或者方法，那就会在这个对象的原型对象中去找，以此类推，直到原型链结束。

## `_proto_`

`_proto_`是原型链查询中实际用到的，指向构造函数的原型对象，他是**对象独有的**。`对象._proto_ = 构造函数.prototype`。

在js中，万物皆是对象，函数也是对象。所以构造函数也会有`_proto_`属性。

## constructor

每个函数都有一个原型对象，该原型对象有一个constructor属性，指向创建对象的函数本身。

## 总结

1、只有函数才有prototype属性。

2、`对象._proto_ = 构造函数.prototype`。

3、构造函数的prototype指向原型对象，原型对象的constructor指向构造函数。

## 使用

prototype最主要的用法就是将属性暴露成公用的。

``` js
function Person(name) {
    this.name = name;
    this.f = function() {
        console.log("name is " + this.name)
    }
}

var wang = new Person("wang");
var li = new Person("li");
console.log(wang.name);			//wang
console.log(li.name);			//li
console.log(wang.f === li.f);	//false
```

虽然wang和li都有f属性，但是实例对象访问的都是自己的私有属性。

使用prototype将f方法变成公共属性：

``` js
function Person(name) {
    this.name = name;
}
Person.prototype.f = function() {
    console.log("name is " + this.name)
}

var wang = new Person("wang");
var li = new Person("li");
console.log(wang.name);			//wang
console.log(li.name);			//li
console.log(wang.f === li.f);	//true
```

这就是prototype最大的作用：将属性或方法暴露为共有。

