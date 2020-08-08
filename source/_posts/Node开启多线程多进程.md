---
title: Node开启多线程多进程
date: 2020-07-17 23:59:07
tags:
---

# Node的多进程和多线程问题

我们知道Node.js是以单线程的模式运行的，但它使用的是事件驱动来处理并发，这样有助于我们在多核cpu的系统上创建多个进程，从而提高性能。

> 面试官：问你Node能开启多线程吗？
>
> 你：No problem!

## 开启多进程

node中开启多进程有两个模块：`child_process`模块的`cluster`模块。

`child_process`模块可以实现子进程，从而实现广义的多进程模式。

在`child_process`模块中提供了四个创建子进程的方法，区别如下：

- `spawn`：子进程中执行的是**非node程序**，提供一组参数后，执行的**结果以流的形式返回**。
- `execFile`：子进程中执行的是**非node程序**，提供一组参数后，执行的**结果以回调的形式返回**。
- `exec`：子进程中执行的是**非node程序**，提供一组**shell命令**，执行的**结果以回调的形式返回**。
- `fork`：子进程中执行的是**node程序**，提供一组参数后，执行的**结果以流的形式返回**。

node中的主进程称为**Master**线程，子进程称为**Worker**进程。在创建子进程之后，父子进程就可以开始进行通信。

> 单个Node.js实例运行在单个线程中。为了充分利用多核系统，有时候需要启用一组Node.js进程去处理负载任务。

`cluster`模块可以创建共享服务器端口的子进程。

工作进程由`child_process.fork()`方法创建，因此它们可以使用IPC和父进程通信，从而使各进程交替处理连接服务。

## 进程之间的通信

在NodeJS中，父子进程之间的通信可以通过`on('message')`和`send()`方法实现通信。`on('nessage')`用来监听message事件，使用`send()`向其他进程发送消息。

> 面试官：多个进程可以监听同一个端口吗

主进程和worker可以监听同一个端口，但是master进程是不会处理具体业务的，因此需要使用worker去处理事务。当网络请求到来的时候，会进行抢占式调度。最后只会有一个master抢到任务并且处理。

除了父子进程之间的通信，还有别的通信方式。大概有如下几种：

- **`stdin/stdout`传递json**。是最直接的方式，适用于关联进程之间的通信，无法跨机器。
- **node原生IPC**。同样的约束。
- **通过sockets**。这是最通用的方式，有良好的跨环境能力，但存在网络性能消耗的问题。
- **借助message queue**。是为通信问题而扩展出的一层强大的消息中间件。

## 开启多线程

`worker_threads`模块允许使用并行地执行JavaScript的线程。

`worker_threads`相对于I/O密集型操作是没有太大的帮助的，因为异步的I/O操作比worker线程更有效率，但对于CPU密集型操作的性能会提升很大。

线程间的通信方式有：

- **共享内存**。线程之间可以共享内存，使用`ArrayBuffer`或`SharedArrayBuffer`。
- **parentPort**。主要用于父子线程通信，通过经典的`on('message')`，postMessage形式。
- **MessageChannel**。创建自定义的消息传递通道。

> 与 [Web 工作线程](http://nodejs.cn/s/skL7X7)和 [`cluster` 模块](http://nodejs.cn/s/4cbAVb)一样，可以通过线程间的消息传递来实现双向通信。 在内部，一个 `Worker` 具有一对内置的 [`MessagePort`](http://nodejs.cn/s/wt864Y)，在创建该 `Worker` 时它们已经相互关联。 虽然父端的 `MessagePort` 对象没有直接公开，但其功能是通过父线程的 `Worker` 对象上的 [`worker.postMessage()`](http://nodejs.cn/s/U2WeKZ) 和 [`worker.on('message')`](http://nodejs.cn/s/mF5JVx) 事件公开的。
>
> 要创建自定义的消息传递通道（建议使用默认的全局通道，因为这样可以促进关联点的分离），用户可以在任一线程上创建一个 `MessageChannel` 对象，并将该 `MessageChannel` 上的 `MessagePort` 中的一个通过预先存在的通道传给另一个线程，例如全局的通道。

## 总结

- 开启多进程使用**`child_process`模块或`cluster`模块，**开启多线程使用**`worker_threads`模块**。
- 进程创建有四个方法**`spawn`、`exec`、`execFile`、`fork`。**
- 进程通信方式有**`stdin/stdout`传递json**、**node原生IPC**、**sockets**、**message queue**。
- 线程通信方式**共享内存、`parentPort`、`MessageChannel`**。

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)