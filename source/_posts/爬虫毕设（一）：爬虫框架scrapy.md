---
title: 毕业设计（一）：爬虫框架scrapy
date: 2020-03-13 00:19:28
tags: 毕业设计
---

## Scrapy安装

安装Scrapy有两种途径：

- 使用pip安装：` pip install Scrapy `
- 使用国内豆瓣安装：` pip install -i https://pypi.douban.com/simple/ scrapy `

推荐使用第二种方式，安装速度很快。

## Scrapy命令

在命令行中输入scrapy，会直接显示常用的命令：

![Scrapy命令](https://img-blog.csdnimg.cn/20200313002011631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
1、`scrapy startproject Demo（项目名）`：创建一个新的项目。

2、`scrapy genspider name domain`：name是爬虫的名字，domain是所爬取的网站名。

3、`scrapy crawl <spider>`：启动爬虫。

4、`scrapy list`：查看所有的爬虫。

5、`scrapy fetch <url>`：打印响应。

6、`scrapy shell [url]`：调试shell。

在后续的系统设计的时候回慢慢的使用到各种命令进行调试。

![文件结构](https://img-blog.csdnimg.cn/20200313002033588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
1、scrapy.cfg：项目的配置文件

2、Spider/spiders：爬虫代码文件

3、Spider/items.py：存储爬取到的数据的容器

4、Spider/pipeline.py：执行保存数据的操作

5、Spider/setting.py：项目的设置文件

6、Spider/middlewares.py：中间件

在写代码的时候需要修改每个文件的内容。

## spider类

spider类，定义爬虫的方法和属性。下边列出常见的方法和属性：

类属性：

- `name`：定义爬虫的名字，在项目中不能重复。
- `allowed_domains`：允许爬取的域名。
- `start_urls`：起始URL列表，允许有多个url地址。
- `custom_settings`：spider的设置，会覆盖全局设置。
- `settings`：运行爬虫的配置。
- `logger`：制定爬虫创建的python logger name，可以用来发送日志消息。

类方法：

- `from_crawler(cls, crawler, *args, **kwargs)`：类方法，用来实例化对象，将它绑定到spider对象。
- `start_requsets(self)`：生成器，返回由URL构造的Request，作为入口，在爬虫运行的时候自动运行。
- `parse(self, reponse)`：解析函数，返回Item或Requests，一直循环到处理完所有的数据。
- `close(self, reason)`：爬虫关闭时自动运行。

## Request对象

 scrapy使用内置的`scrapy.http.Request`与Response对象去请求网络资源与响应的处理 ，常见的request对象参数列表：

- `url`：请求页面的url地址
- `callback`：回调函数，也就是页面解析函数
- `method`：http请求的方法，默认'get'
- `headers`：http请求的头部信息
- `body`：http请求的正文
- `cookies`：cookie
- `encoding`：编码，默认utf-8

## Response对象

Response类用于http下载返回信息的类，它只是一个基类，他还有几个子类：

- TextResponse
- HtmlResponse
- XMLResponse

当一个页面下载完成，下载器根据http响应头部中的Content-Type字段创建某个Response子类对象。Response对象属性和方法：

- `url`：响应的url字符串
- `status`：响应的http状态码
- `body`：响应的正文
- `request`：返回请求此响应的Request对象
- `meta`：元数据
- `copy()`：返回一个新的Response，它是此Response的副本

这就是爬虫的大概内容，后边的代码会在做毕业设计的过程中一步步的做完。

![个人微信公众号](https://img-blog.csdnimg.cn/20200402001106322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)