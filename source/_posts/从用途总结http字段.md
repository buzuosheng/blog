---
title: 从用途总结http字段
date: 2020-10-04 23:00:34
tags: 面试
description:
categories: 面试
---

网络通信是前端必知必会的一部分内容，同时也是面试中出现的高频问题。

HTTP是**超文本传输协议**的缩写，HTTP协议采用了**请求/响应模型**。所以一般分为**通用、请求、响应**三类的头字段。在DevTools中的Network面板中，随意点一个请求，就可以看见General、Response Headers、Request Headers。

但这篇文章从另一个角度来分类。这样有一个好处，在面试中，可以从各种角度绕回到http头字段。

## 信息类

主要规范接受的字符编码、编码格式、内容类型等。主要有以下字段：

- Accept：接受的内容类型
- Accept-Charset：字符编码
- Accept-Encoding：编码格式
- Accept-Language：语言
- From：发送请求的用户的Email地址
- Date：发送报文的日期和时间
- Status-Code：状态码
- ...

## 缓存

大家都知道缓存策略有两种：**强缓存和协商缓存**，且都是通过HTTP Header来实现的。

强缓存**在缓存时间内直接从缓存中获取数据**，状态码为200。设置两种HTTP Header：

- `Expires`：值为一个时间，代表资源失效时间。当前时间超过该时间时，缓存失效。
- `Cache-Control`：其中一个字段`max-age=30`表示当前资源的有效时间为30s。

协商缓存则需要**验证请求资源是否有更新**，如果命中缓存则状态码为304。也是设置两种HTTP Header：

- `Last-Modified`表示本地文件最后修改时间。
- `If-Modified-Since`会将上边的值发送给服务器，询问服务器在该日期后资源是否有更新。
- `Etag`类似于文件指纹，当文件改变后Etag值就会发生改变。
- `If-None-Match`将上边的值发送给服务器，询问服务器资源是否发生改变。

## 网络通信

HTTP通信是无状态的，但在与服务器通信时是需要有状态的。cookie是服务器保存在客户端的一小段文本信息，大小不超过4K。浏览器每次向客户端发送请求的时候，都会自动附带这段信息。

在Cookie中也有很多字段

- **name**：cookie的名称
- **value**：cookie的值
- **domain**：cookie可以访问的域名
- **path**：cookie可以访问的页面路径
- **Size**：cookie的大小
- **httponly**：设置js禁止访问cookie，可以防止XSS攻击
- **expires/Max-Age**：有效期
- **secure**：设置是否只能通过https传递该cookie

面试又可以扯

## 跨域

如果浏览器发送请求时，如果存在跨域行为，则会在请求头中携带**Origin**字段。

同时，服务器会在响应报文中添加**Access-Control-Allow-Origin**字段，值为允许跨域的域名。

在跨域问题又可以聊跨域的种解决方式：jsonp、cors、Nginx等九种。

使用`vary:origin`防止CDN破坏

## 网络安全

XSS是一种最常见且危害最大的网页安全漏洞，攻击者想尽一切办法。

CSP可以极大程度的保证网页的安全性。

开启CSP的一种方式就是HTTP Header的`Content-Security-Policy`字段。

CSP也是内含了多个字段，对浏览器的不同类型的资源做出限制。

面试的时候又可以再扯其他的网络安全问题及解决方式。

