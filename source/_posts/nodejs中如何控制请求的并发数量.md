---
title: nodejs中如何控制请求的并发数量：async-pool
date: 2020-10-25 23:16:00
tags: 面试
description:
categories: npm包
---

面试问题：

> 现在有10个请求，如何做到同时处理三个请求。

大体思路：

设置当前请求数和最大限制，当请求数小于限制，继续发送请求；当请求数大于限制时，将请求压入请求队列。当一个请求收到响应后，在请求队列头弹出一个请求发送。

代码实现：

``` js
concurrent = (limit, array, iteratorFn) => {
  let i = 0;
  const ret = []
  const executing = []
  const enqueue = function () {
    // 边界处理
    if ( i === array.length()){
      return Promise.resolve()
    }
    
    // 每当一个请求处理完成，就初始化一个promise实例
    const item = array[i++]
    const p = Promise.resolve().then(() => {iteratorFn(item, array)})
    // push到promise数组
    ret.push(p)
    // 删除执行完的promise
    const e = p.then(() => executing.splice(executing.indexOf(e), 1))
    executing.push(e)
    
    let r = Promise.resolve()
    if (executing.length >= limit) {
      // 最先处理完的请求
      r = Promise.race(executing)
    }
    // 递归
    return r.then(() => enqueue)
  };
  // 返回所有的处理结果
  return enqueue().then(() => Promise.all(ret))
}
```

代码流程如下：

1、首先遍历array数组，从第一个元素开始，实例化promise对象。

2、当promise达到上限时，使用Promise.race()获取executing中最先处理完的。

3、删除处理完的请求，实例化新的promise。

4、直到遍历完整个目标请求数组，调用Promise.all()返回。

这里是一个叫`async-pool`第三方库，可以实现相同功能的还有`es6-promise-pool`和`p-limit`等库，可以使用npm安装，看源代码深入理解。

同时还有一篇文章：

[如何对Promise限流：实现一个Promise.map](https://juejin.im/post/6844903907651485710)：https://juejin.im/post/6844903907651485710