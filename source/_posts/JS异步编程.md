---
title: JS异步编程
date: 2020-03-07 22:50:28
tags: 面试
categories: 面试
---

## 什么是异步

同步（sync）是一件事一件事的执行，只有前一个任务执行完毕才能执行后一个任务。异步（async）相对于同步，程序无须按照代码顺序自上而下的执行。

## 为什么要使用异步

由于js是单线程的，只能在js引擎的主线程上运行，所以js代码只能一行一行的执行，如果没有异步的存在，由于当前的任务还没有完成，其他的所有操作都会无响应，用户就会长时间的在等待。

## JS常见的异步模式

常见的异步模式有六种：

- 回调函数
- 事件监听
- 发布/订阅模式
- promise
- Generator(ES6)
- async/await(ES7)

### 回调函数

回调函数是异步操作最基本的方法。

回调函数作为参数传递给另一个函数，在另一个函数中被调用。常见的回调函数的例子：

``` js
ajax(url, () => {
    //处理逻辑
})
```

但是使用回调函数，经常会写出回调地狱，这是非常致命的。

回调地狱的根本问题是：

- 嵌套函数存在耦合性
- 嵌套函数变多，处理问题的困难也变大

### 事件监听

事件监听模式，异步任务的执行取决于，某个事件的发生。比如点击事件（onClick）和内容改变时间（onChange）等。

### 发布/订阅模式

在发布/订阅模式中，想象有一个类似消息中心的地方，可以在消息中心“注册”一条消息，然后就会有若干对这消息感兴趣的人“订阅”，一旦该消息被“发布”，所有”订阅“了该消息的用户都会得到提醒。

### Promise

Promise是ES6推出的一种解决异步编程的解决方案。Promise是承诺的意思，这个承诺在未来会有一个确定的答复，该承诺有三种状态：等待中（pending）、完成了（resolved）、拒绝了（rejected）。一旦状态从等待改变为其他状态就不再可变了。

Promise是个构造函数，接受一个函数作为参数。作为参数的函数有两个参数：resolve和reject，分别对应完成和拒绝两种状态。我们可以选择在不同时候执行resolve或reject去触发下一个动作，执行then方法里的函数。

``` js
ajax(rul)
  .then(res => {
    console.log(res)
    return ajax(url1)
  }).then(res => {
    console.log(res)
    return ajax(url2)
  }).then(res => console.log(res))
```

Promise实现了链式调用，每次调用then之后返回的都是一个Promise对象，如果在then使用了return，return返回的值会被`Promise.resolve()`包装。

### Generator

Generator是一种特殊的函数，有以下特点：

- 声明时需要在function后面加上*，并且配合函数里面yield关键字使用。
- 在执行Generator函数的时候，会返回一个Iterator遍历器对象，通过其next方法，将Generator内的代码以yield为分界分步执行。
- 执行Generator函数时，代码不会执行，而是需要调用Iterator遍历器对象的next方法，这时程序才会执行**从头或从上一个yield到下一个yield或return或函数体尾部之间的代码**，并将yield后面的值包装成json对象返回。
- value取的yield或return后面的值，否则就是undefined，done的值如果碰到return或者执行完函数体会返回true，否则就会返回false。

### Async/Await

一个函数如果加上async，那么该函数就会返回一个Promise对象。

``` js
async function test() {
    console.log('1')
}
console.log(test)	// Promise {<resolve>: "1"}
```

async就是将函数返回使用`Promise.resolve()`，和`then`处理返回值一样，await只能配套async使用。但如果多个异步代码没有依赖性却使用了await会导致性能降低。

async在使用上会有一些需要注意的地方：

- async函数的返回值是一个Promise对象，不像是generator函数返回的是Iterator遍历器对象，所以async函数执行后可以继续使用then等方法来继续执行后面的逻辑。
- await后边一般跟Promise对象，async函数执行遇到await后，等待后面的Promise对象的状态从pending变成resolve后，将resolve的参数返回并自动往下执行知道下一个await或结束。
- await后面也可以跟一个async进行嵌套使用。

## Event Loop

JavaScript是一门单线程语言，同一时间只能做一件事情。在js中有两类任务：

- 同步任务
- 异步任务

在js主线程中的任务执行：

1、同步和异步任务分别进入不同的“场所”执行。所有同步任务都在主线程上执行，形成一个执行栈，而异步任务进入Rvent Table并注册回调函数。

2、当这个异步任务有了运行结果，Event Table会将这个回调函数移入Event Queue，进入等待状态。

3、当主线程同步任务执行完成，会失去Event Queue读取对应的函数，并结束它的等待状态，进入主线程执行。

4、主线程不断重复上面3个步骤，也就是常说的Event Loop（事件循环）

## 宏任务和微任务

除了广义的同步任务和异步任务，任务还有更精细的定义：

- 宏任务（macro-task）：包括整体代码script、setTimeOut、setInterval、I/O、UI交互事件，**可以理解是每次执行栈执行的代码就是一个宏任务**。
- 微任务（micro-task）：Promise，process.nextTick，且process.nextTick的优先级大于Promise.then，**可以理解是在当前task执行结束后立即执行的任务**。

事件循环的顺序，决定js代码的执行顺序。进入整体代码（宏任务）后，开始第一次循环，接着执行所有的微任务，然后再从宏任务开始，找到其中一个任务队列执行完毕，在执行所有的微任务。

`setTimeOut(fn, 0)`在下一轮事件循环开始时执行，`Promise.then`在本轮事件循环结束时执行。

不同类型的任务会进入对应的Event Queue：

Promise中的异步体现在`then`和`catch`中，所以写在Promise中的代码是被当做同步任务执行的。

await实际上是让出线程的标志。**await后面的表达式会先执行一遍，将await后面的代码加入到microtask中**，然后就会跳出整个async函数来执行后面的代码。

由于async/await本身是promise+generator的语法糖，所以await后面的代码是microtask。

## Node中的Event Loop

Node中Event Loop和浏览器中的完全不同。

Node的Event Loop分为六个阶段，它们会按照**顺序**反复执行。每当进入一个阶段的时候，都会从对应的回调队列中取出函数去执行。当队列为空或者执行的回调函数数量到达系统设定的阈值，就会进入下一阶段。

- timer：这个阶段会执行`setTimeout`和`setInterval`回调，并且是由poll阶段控制的。
- I/O：这个阶段会处理一些上一轮循环中**少数未执行**的I/O回调。
- idle，prepare
- poll：这是一个至关重要的阶段。这一阶段系统会做两件事：
  - 回到timer阶段执行回调。
  - 执行I/O回调
- check：这个阶段执行`setImmdiate`
- close callback：执行close事件

![个人微信公众号](https://img-blog.csdnimg.cn/20200402001106322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)