---
title: 阿里云OSS托管应用
date: 2020-08-12 11:26:50
tags: 前端工程化
categories: 前端工程化
---

# 使用Ali OSS托管项目

一般写完前端项目后，就是将自己的项目打包部署了。我使用过[vercel](https://vercel.com/dashboard)和[Netlify](https://www.netlify.com/)两个网站托管平台。今天使用阿里云的OSS（对象存储服务）来托管自己的博客。

自己的博客是使用[Hexo](https://hexo.io/zh-cn/)搭建的，博客主题使用了[fexo](https://github.com/forsigner/fexo)，从代码上传到Github之后开始讲起。

## Github Actions

工作流程是可以在Github仓库中创建的自定义自动化流程，用于在Github上构建、测试、封装、发行、或部署任何代码项目。

Github Actions可以直接在仓库中构建端到端**持续集成（CI）和持续部署（CD）**功能。工作流程在Github托管的计算机上的Linux、macOS、Windows和容器中运行。

在Github中已经有别人造好的轮子，所以有一些Actions，我们可以直接引用别人写好的action。

你的代码很不错，可下一秒就是我的了。

![这辈子都不可能造轮子的](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1597162486393&di=0707203947a87d85bf573e22bb3c6365&imgtype=0&src=http%3A%2F%2Fcms-bucket.ws.126.net%2F2020%2F0422%2F039fb585j00q95g1c003oc000go00akc.jpg)

工作流程文件放在`.github/workflows`目录下，文件格式为yaml格式，后缀名为`.yml`

先贴我的代码，然后再做解释。

``` yaml
name: deploy blog

on:
  push:
  schedule:
    - cron: '30 20 * * *'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
      with:
        submodules: true
    - uses: srt32/git-actions@v0.0.3
      with:
        args: git submodule update --init --recursive
    - name: use Node.js 10.x
      uses: actions/setup-node@v1
      with:
        node-version: 10.x
    - name: npm i hexo
      run: npm install hexo-cli -g
    - name: npm install
      run: npm install
    - name: npm run build
      run: hexo g
    - name: setup aliyun oss
      uses: manyuanrong/setup-ossutil@master
      with:
        endpoint: oss-cn-beijing.aliyuncs.com
        access-key-id: ${{ secrets.OSS_KEY_ID }}
        access-key-secret: ${{ secrets.OSS_KEY_SECRET }}
    - name: cp aliyun oss
      run: ossutil cp -rf public oss://buzuosheng-blog
```

在Github上的事件发生后触发工作流程，在工作流程名称后边加上`on: 事件值`。

``` yaml
name: deploy blog
on: push
```

计划工作流程的运行，在文件流程中使用POSIX cron语法。

``` yaml
on:
  schedule:
    - cron: '30 20 * * *'
```

上述代码表示在每天的20：30触发一次。

一个workflow可以包含一个或多个job；一个job可以包含一个或多个step；一个step可以包含一个或多个action。

`runs-on: ubuntu-latest`代表这个工作流程运行在Ubuntu系统。

在工作流程中引用操作：

``` yaml
    - uses: actions/checkout@v1
      with:
        submodules: true
```

**[在工作流程中使用变量和密码](https://docs.github.com/cn/actions/configuring-and-managing-workflows/using-variables-and-secrets-in-a-workflow)**需要在仓库设置中添加Secrets。

![使用变量和密码](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQWJ0ZkpZaWM1eWVmRzJtajczS3V3NXhqWkRYYks0Y3RYYWhpYXdybm1aaEROaFVNMDJTV3I2R2ZWb1FtSlRuTzg1ZjQxQmljWDl0NTlvZy82NDA.png)

之后在使用`npm install`等命令下载各种包和工具，打包生成静态文件就OK了。

![工作流程](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQW9ndGZRQkxwcEJpYmNoMGliY3ZzV3BDMXEyZEpwUzlka3JVV3NyY21leXludnJoTkRtMFBXb3JUa0MzYWliNXpHMWFTOVpaVUZwNVpqUS82NDA.png)

## Ali OSS

[阿里云对象存储服务](https://oss.console.aliyun.com/overview)是阿里云提供的海量、安全、低成本、高可靠的云存储服务。需要先创建一个Bucket，然后将读写权限设置为**公共读**，静态页面首页设置为`index.html`，404页面设置为`404.html`。之后就可以上传文件了。

![使用ossutil上传文件](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQW9ndGZRQkxwcEJpYmNoMGliY3ZzV3BDdDJGM2xkdTJyb3ZiNXdpYmFhbGVYcnBqbDcwYzRSNlZEZGVEbzJ4Mk1pY3dVbzhzN05YbTd1cncvNjQw.png)

虽然可以直接点上传文件的按钮上传文件，但是也太不利于自动化了，而且显得特别外行。

这时候就要用[ossutil](https://help.aliyun.com/document_detail/50452.html)工具了，使用ossutil将文件上传到oss。

上传文件主要用到`cp`命令。

``` yaml
# ./ossutil cp file_name cloud_name [-r] [-f] [-u]
  - name: 上传文件到阿里云
    run ossutil cp -rf public oss://buzuosheng-blog
    
# 参数说明
-r, --recursive 递归进行操作，上传文件夹
-f, --force 	强制操作，不进行询问提示
-u, --update 	更新操作，文件更新过以后才会执行上传/下载/拷贝操作
```

在使用ossutil上传文件时需要先创建`access key`。如何创建[AccessKey](https://help.aliyun.com/document_detail/53045.html)。

## 开通CDN

开通CDN之后，可以加快网站的访问速度。

![添加CDN](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQW9ndGZRQkxwcEJpYmNoMGliY3ZzV3BDY0Rjd0RTeExLZUV5d0FDbFU5RGZRbEVqRlJ5UnhZaWNmVUZLNnM5aWFKRHFJVFY0OXQweWxHQ2cvNjQw.png)

在oss的Bucket的域名管理中绑定域名，配置CDN加速。一路确定点点点。

然后再添加HTTPS证书，点点点。

![添加HTTPS](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQW9ndGZRQkxwcEJpYmNoMGliY3ZzV3BDbEF3Z01HRk95YVVqamNzb0xXTzFpY2lhbTBxTzVXWUFZMXZzNzVuVVhpYmFYT2NVaGlhNnF1bEZNUS82NDA.png)

之后再在阿里云域名添加域名解析到该地址。[如何配置域名解析](https://help.aliyun.com/document_detail/27144.html)。

![域名解析](aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HWTlaSlB4NmJNQW9ndGZRQkxwcEJpYmNoMGliY3ZzV3BDTldkTUlYRHpueGVpYVRGaWJ4Sk8wSWphc2FGZlNuM0dEMXFZaWF5bDdBRmUzdXBkTkxPUWdqdW1BLzY0MA.png)

## 总结总结

- 在代码仓库中添加工作流程文件；
- 项目push到Github仓库时，push事件会被监听到；
- Github运行workflow，将项目打包生成静态文件；
- 使用ossutil将打包好的静态目录上传到Ali OSS；
- 域名解析到自己的网址。

之后就可以在自己的域名访问到自己的博客[不作声的博客](http://buzuosheng.com)。

