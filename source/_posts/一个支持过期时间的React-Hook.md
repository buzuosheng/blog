---
title: 一个支持过期时间的localStorage React Hook
date: 2021-03-06 18:58:32
tags: ['React Hooks', 'localstorage', '钩子函数', 'npm', '过期时间']
description: 支持过期时间的localStorage React hook
categories: React
---

最近自己造了一个轮子，支持过期时间的localStorage React Hook。

localStorage只有`getItem`, `setItem`, `removeItem()`, `clear()`4个API，本身并不支持过期时间，但我们可以添加这个功能并封装成React Hook函数。

实现思路如下：

- 将Item的值和过期时间作为一个对象，使用`JSON.stringify()`处理
- 调用`setItem()`，将值和过期时间一起存入
- 调用`getItem()`，用`JSON.parse()`处理取出过期时间，与当前时间判断
- 如果过期调用`removeItem()`删除缓存，如果没过期就获取Item的值

最后打包发布到npm。

![image-20210306192716649](image-20210306192716649.png)