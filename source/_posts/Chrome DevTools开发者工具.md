---
title: Chrome DevTools开发者工具
date: 2020-03-15 00:41:28
tags: 
---

Chrome DevTools是内嵌在Chrome浏览器里的一组用于网页制作和调试的工具。使用DevTools，可以在平时中的开发调试中极大的提高效率。

使用快捷键`ctrl+shift+i`或者`f12`可以直接打开开发者工具。

在DevTools开发者工具一共有九个部分。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005714556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
## Element
![Element](https://img-blog.csdnimg.cn/20200315005627216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
在这张图中，右边的开发者工具分为两部分，左边为HTML代码，右边是CSS代码以及样式。

1、最基本的使用就是快速定位到DOM节点在网页中的位置，或者DOM节点在在HTML代码中的位置。

在开发者工具模式下，鼠标移动到HTML代码上，网页上对应的DOM节点会亮起来，还会显示一些简单的信息。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005740292.png)

反过来，通过DOM元素定位HTML代码位置，只需要先点击下图这个按钮，然后鼠标移动到DOM元素上的时候，HTML代码会直接移动到DOM元素对应点代码位置。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005807562.png)

2、在HTML代码区域和CSS样式区域，可以直接双击鼠标编辑代码，样式会直接在页面上显示。

3、直接向元素添加不同状态下的样式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005833154.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

4、在Event Listeners中查看元素绑定的事件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005849329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

5、在HTML代码中，右键元素，可以对改元素进行操作。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005908696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

6、`ctrl+f`搜索，可以在网页上搜索内容，也可以在HTML中搜索。

## Console

在控制台能显示浏览器的消息。(百度首页的控制台信息下有直接递交简历的地址)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005923719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

可以在控制台执行代码段。

## Sources

这里是源代码面板。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005946471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

1、可以在每一行代码中设置断点调试代码。

2、这里可以查看网页的源代码文件，在下边有一个`{}`图标，点击以后可以格式化代码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315005959645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

## Network

Network也是很重要的一部分，功能也有很多。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200315010011459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)

1、“Disable cache"选项控制是否缓存，防止更改样式而看不到效果。

2、“Request Table”中可以显示可下载资源。（网页中的可下载视频找不到下载按钮，可以在这里下载）

3、filter可以过滤不同类型的资源，支持正则表达式过滤。

4、在“Name”点击一个request请求，会显示出request、response、cookie等信息。

## Performance

## Memory

## Application

查看缓存数据库等。

## Security

Security 面板可以区分两种非安全页面。
如果请求的页面通过 HTTP 提供，则主源会被标记为不安全。
如果请求的页面通过 HTTPS 检索，但页面会继续使用 HTTP 检索其他源的内容，然后页面仍然会被标记为不安全。 

## Audits

![个人微信公众号](https://img-blog.csdnimg.cn/20200402001106322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)