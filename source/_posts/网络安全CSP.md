---
title: 网络安全CSP
date: 2020-10-04 13:57:06
tags: [浏览器, 面试]
description: 内容安全策略CSP
categories: 面试
---

## 网络安全防范

XSS的全称叫做**跨域脚本攻击**，是最常见且危害性最大的网页安全漏洞。

简单来说，就是**攻击者想尽一切办法将可执行的代码插入到网页中**。

比较常见的是在评论功能中，攻击者可以在评论区提交以下评论：

``` html
<script>alert(1)</script>
```

该评论被提交后，会存储在数据库中，当其他用户打开该页面时，该代码会被自动执行，用户就会被攻击到。

因此当我们在写代码的时候，必须时刻提防，对用户的输入永远保持不可信的态度。

为了防止XSS，我们在每次做项目的时候，做大量的编程措施。但这会做大量重复的工作，非常麻烦，显然不符合代码可重用可复用的理念。

有人就说了啊，能不能把这个工作交给浏览器去处理呢，让浏览器自动禁止外部恶意脚本？

然后**网页安全策略（CSP）**就出现了。

## CSP

CSP的**本质是添加白名单**，开发者告诉客户端，哪些外部资源可以加载和执行，也就是添加白名单。客户端负责提供配置，实现和执行全部都交给浏览器。

开启CSP之后，网页的安全性得到了极大的保障。

开启CSP有两种方式。一种是通过**HTTP头信息的`Content-Security-Policy`字段**，另一种是通过**网页的`meta`标签**。

第一种可以在DevTools中的Network面板中查看：

![github中的CSP设置](0.png)

第二种如下：

``` html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:">
```

开启之后，不符合CSP的外部资源就会被阻止加载。

## CSP字段

CSP通过不同的字段限制不同类型的资源。

- script-src：外部脚本
- style-src：样式表
- img-src：图像
- media-src：音频和视频
- font-src：字体
- object-src：插件
- child-src：框架
- frame-ancestors：嵌入的外部资源``<frame>``
- connect-src：HTTP连接
- worker-src：worker脚本
- manifest-src：manifest文件

默认配置使用`default-src`，默认配置即为各个字段的默认值。

有时候，我们除了防范XSS攻击，还希望可以记录此类行为，可以使用`report-uri`字段。

更多详细配置可以参考[阮一峰 Content Security Policy 入门教程](http://www.ruanyifeng.com/blog/2016/09/csp.html)。