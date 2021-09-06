---
title: JS进阶面试题
date: 2020-03-10 15:25:28
tags: 面试
categories: 面试
---

## call()

`call(obj,arg1,arg2,arg3)`

作用：可以改变this的指向。

参数：第一个参数是this的指向对象，第二个参数起依次传入给函数的参数值。

实现分析：

- `context`设置为可选参数，如果不传默认为`window`
- 给`context`创建一个`fn`属性，并将值设置为需要调用的函数
- 将`call`的多个参数剥离
- 调用函数并将对象上的函数删除。

``` js
Function.prototype.myCall = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  context = context || window
  context.fn = this
  const args = [...arguments.slice(1)]
  const result = context.fn(...args)
  delete context.fn
  return result
}
```

## apply()

`apply(obj, args)`

作用：改变this的指向。

参数：args，所有的参数放在一个数组里，作为apply接收的第二个参数

实现分析：只是参数处理与call不同。

``` js
Function.prototype.myApply = function(context) {
  if (typeof this !== 'function') {
      throw new TypeError('Error')
  }
  context = context || window
  context.fn = this
  let result
  //处理参数
  if (arguments[1]) {
    result = context.fn(...argument[1])
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```

## bind()

作用：改变this指向

参数：与call()相同

实现分析：需要返回一个函数

``` js 
Function.prototype.myBind = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  const _this = this
  const args = [...arguments].slice(1)
  
  //返回一个函数
  return function F() {
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```

## new()

在调用`new`的过程中发生了以下四件事：

1、新生成一个对象

2、链接到原型

3、绑定this

4、返回新对象

实现分析：

- 创建一个空对象
- 获取构造函数
- 设置空对象的原型
- 绑定`this`并执行构造函数
- 确保返回值为对象

``` js
function myNew() {
  let obj = {}
  let Con = [].shift.call(arguments)
  obj._proto_ = Con.protorype
  let result = Con.apply(obj, arguments)
  return result instanceof object ? result : obj
}
```

使用`new Object()`的方式创建对象需要通过作用域链一层层找到`Object`，但是使用字面量的方式就没有这个问题。

## instanceof

`instanceof`可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的`prototype`。

实现分析：

- 获取类型的原型
- 获取对象的原型
- 一直循环判断对象的原型是否等于类型的原型，知道对象原型为null，因为原型链最终都会null

``` js
function myInstanceof(left, right) {
  let prototype = right.prototype
  left = left._proto_
  while (true) {
    if (left === null || left === undefined)
      return false
    if (prototype === left)
      return true
    left = left._proto_
  }
}
```

## 0.1 + 0.2 != 0.3

js采用IEEE 754 双精度版本（64位），计算机都是通过二进制来储藏数据的，在二进制中0.1表示为：

``` js 
0.100000000000000002 === 0.1 // true
```

0.2和0.1在二进制中同样是无限循环的：

``` js 
0.200000000000000002 === 0.2 // true
```

所以两者相加：

``` js
0.1 + 0.2 === 0.300000000000000004 // true
```

在`console.log(0.1)`的时候，二进制又被转换为十进制，所以输出结果为`0.1`：

``` js
console.log(0.1)	// 0.1
console.log(0.100000000000000002)	// 0.1
```

## V8垃圾回收机制

V8将内存分为新生代和老生代两部分内存空间，新生代内存空间主要用来存放生存时间较短的对象，老生代内存空间主要用来存放存活时间较长的对象。对于垃圾回收，新生代和老生代各自策略不同。

### 新生代垃圾回收

新生代内存中的垃圾回收主要通过Scavenge算法进行，具体实现时采用了Cheney算法。Cheney算法将内存空间一分为二，每部分都叫做一个Semispace，一个处于使用，一个处于闲置。处于使用中的Semispace也叫作From，处于闲置中的Semispace也叫作To。

新分配的对象会被放入From空间，当From空间被占满时，新生代GC就会启动。需要回收的对象留在From，剩下的对象移动到To空间，然后进行反转，将From和To空间互换，进行垃圾回收时，会将To空间的内存进行释放。

 ![img](https://upload-images.jianshu.io/upload_images/3831834-e536b4847cb877c7.png?imageMogr2/auto-orient/strip|imageView2/2/w/578/format/webp) 

### 新生代对象的晋升

新生代中的对象可以晋升到老生代中，具体有两种方式：

1、在垃圾回收的过程中，如果发现某个对象之前被清理过，那么就会将其晋升到老生代内存空间中。

2、在From空间和To空间进行反转的过程中，如果To空间的使用量已经超过了25%，那么就将From中的对象直接晋升到老生代内存空间中。

### 老生代垃圾回收

老生代的内存空间是一个连续的结构。老生代内存空间中的垃圾回收有标记清除和标记合并两种方式。

以下情况会优先启用标记清除算法：

- 某一个空间没有分块的时候
- 空间中被对象超过一定限制
- 空间不能保证新生代中的对象移动到老生代中

![个人微信公众号](https://img-blog.csdnimg.cn/20200402001106322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)