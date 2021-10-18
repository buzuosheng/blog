---
title: 浅谈React Fiber
date: 2021-09-24 00:13:24
tags: React
description:
categories: React
---

# React Fiber是什么

先贴一下GIthub的文档：

> React Fiber is an ongoing reimplementation of React's core algorithm. It is the culmination of over two years of research by the React team.
>
> The goal of React Fiber is to increase its suitability for areas like animation, layout, and gestures. Its headline feature is **incremental rendering**: the ability to split rendering work into chunks and spread it out over multiple frames.
>
> Other key features include the ability to pause, abort, or reuse work as new updates come in; the ability to assign priority to different types of updates; and new concurrency primitives.

Fiber是一次对核心算法的重新实现，主要特性是**增量式渲染**，另外的关键特性包括**更新时暂停、中断、重启工作，为不同类型的资源分配优先级，新的并发原语。**

