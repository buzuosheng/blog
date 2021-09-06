---
title: 前端安全问题
date: 2020-03-29 23:50:28
tags: 面试
categories: 面试
---

在互联网时代，信息安全成为一个非常重要的问题，所以我们西部了解前端的安全问题，并且知道如何去预防、修复安全漏洞。

在前端有几种常见的攻击方式：XSS、CSRF、点击劫持、中间人攻击、SQL注入、OS命令注入。

## XSS攻击

**什么是XSS攻击？**

XSS全称Cross Site Scripting，跨站脚本攻击，为了跟CSS区分，所以叫XSS。简单地说，X**SS就是攻击者将恶意脚本注入到网页中**，当用户浏览该网页时，嵌入到Web里的脚本代码就会被执行，对用户浏览器进行控制或获取到用户的隐私数据。

**XSS攻击类型有哪些？**
1、非持久型XSS（反射型XSS）
非持久型XSS，一般是通过给别人发送**带有恶意脚本代码参数的URL**，诱导用户访问该链接，然后恶意代码被HTML解析执行。

2、持久型XSS（存储型XSS）

持久型是恶意代码**被服务端写入到数据库中**，这样当每个浏览器请求数据时都会被攻击到。持久型XSS攻击不需要诱骗点击，黑客只需要在提交表单的地方完成注入即可，但是这种XSS攻击的成本相对很高。

**如何防御XSS攻击？**

通常使用两种方式来防御XSS攻击：

**1、转义字符**

对用户的输入应该永远保持不信任态度，最普遍的做法就是转义输入输出的内容，对于引号、尖括号、斜杠进行转义：

``` js
function a(str) {
  if(!str) return ''
  str = str.replace(/&/g, '&amp;')
  str = str.replace(/</g, '&lt;')
  str = str.replace(/>/g, '&gt;')
  str = str.replace(/"/g, '&quto;')
  str = str.replace(/'/g, '&#39;')
  str = str.replace(/`/g, '&#96;')
  str = str.replace(/\//g, '&#x2F;')
  return str
}
```

**2、CSP**

CSP全称Content Security Policy，内容安全策略，用于**指定哪些内容可执行**，也就是添加白名单，它是一个http头。

开启CSP的两种方式：

- 设置http header中的`Content-Security-Policy`
- 设置`meta`标签的方式`<meta http-equiv="Content-Security-Policy">`

http header举栗：

- 只允许加载本站资源

`Content-Security-Policy: default-src 'self'`

- 只允许加载https协议图片

`Content-Security-Policy: img-src https://*`

- 允许加载任何来源框架

`Content-Security-Policy: child-src 'none'`

## CSRF

**什么是CSRF？**

CSRF是Cross Site Request Forgery，跨站请求伪造，是一种常见的Web攻击。原理是**构造一个后端请求地址，诱导用户点击或通过某种途径自动发起请求。如果用户是在登陆状态下，后端就会认为是用户在操作，从而完成非法操作**。

![CSRF攻击原理](C:\Users\1\Desktop\CSRF.png)

攻击过程为1、用户登陆A网站；2、A网站确认身份；3、B网站向A网站发送请求

**如何防御CSRF攻击？**

SCRF攻击有以下几种防范措施：

- 禁止第三方网站带Cookies
- 在前端页面加入验证信息
- 禁止第三方网站请求
- Get请求不对数据进行修改

## 点击劫持

**什么是点击劫持？**

点击劫持是一种**视觉欺骗的攻击手段**。它的原理是通过iframe标签嵌套，然后将其透明度设置为0，在页面透出一个按钮诱导用户点击。

（个人经历，在手机网页端看小说的时候，点击下一章，然后就会跳转到另一个页面，返回原网页，长按就会显示图片的大小，几乎覆盖了整个页面）

**防御措施**

- JavaScript禁止内嵌
- 请求头`X-FRAME-OPTIONS`禁止内嵌

`X-FRAME_POTIONS`是一个http响应头，有三个值可选：

- DENY，表示页面不允许通过iframe的方式展示
- SAMEORIGIN，表示页面可以在相同域名下通过iframe展示
- ALLOW-FROM，表示页面可以允许指定来源的通过iframe展示

## 中间人攻击

中间人攻击是**攻击方同时与客户端和服务端建立连接**，并让双方认为连接是安全的。中间人监控客户端和服务端之间的通信，同时也能获取到传输的信息。

防御中间人攻击，只需要增加一条安全通道传输信息。

## SQL注入攻击

SQL攻击，可以执行恶意SQL语句。它通过将任意SQL代码插入数据库查询，使攻击者能够完全控制web应用程序后边的数据库服务器。

SQL注入攻击可以通过多种方式执行：

- 带内注入
- 盲注入
- 带外注入

**防范措施：**

- 不要使用动态SQL
- 不要讲敏感数据保留到纯文本中
- 限制数据库权限和特权
- 避免直接向用户显示数据库错误
- 对方问数据库的Web应用程序使用Web应用程序防火墙（WAF）
- 定期测试与数据库交互的Web应用程序
- 将数据库更新为最新的可用修补程序

## OS命令攻击

OS命令注入攻击是指**通过web应用，执行非法的操作系统命令达到攻击的命令**。只要在能调用Shell函数的地方就存在被攻击的风险。

由于能够获取直接执行系统命令的能力，所以OS命令注入攻击之后，基本上可以“为所欲为”。

**防御措施**

- 使用execFile/spawn
- 白名单校验

![个人微信公众号](https://img-blog.csdnimg.cn/20200402001106322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)