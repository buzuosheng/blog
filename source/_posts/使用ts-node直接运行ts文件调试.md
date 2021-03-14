---
title: 使用ts-node直接运行ts文件调试
date: 2021-03-14 22:19:51
tags: ['ts', 'typescript', 'ts脚本', 'ts-node']
description: 记使用ts-node直接运行文件调试，以及各种报错
categories: 'Typescript'
---

## 应用场景

在代码日常中，经常会需要写各种脚本，今天使用ts写了个脚本，运行的时候各种报错，还是决定写下来。

运行ts脚本需要一个库`ts-node`，这个库不能全局安装，否则会报错。

``` powershell
yarn add -D ts-node
# 或者
npm i ts-node -D
```

## 使用

安装好后开始添加配置项：

- 在`ts.config.json`中添加配置`"mudoule": esnext`或`es2005`
- 在`package.json`中添加配置`"type":"modules" `
- 在文件中的`import`语句中**包含文件扩展名**，如`import data from './data'`改为`import data from './data.js'`，另外`.ts`后缀也要改为`.js`

然后就可以使用命令行命令运行ts脚本。

``` powershell
node --loader ts-node/esm ./my-script.ts
```

## 报错

在这个过程中报错不少，在网上各种论坛跑来跑去，终于解决了问题。

> `SyntaxError: Cannot use import statement outside a module`

无法在模块外使用import，解决这个问题需要在`package.json`文件中添加`"type":"modules" `。

> `Error [ERR_MODULE_NOT_FOUND]: Cannot find module 'C:\Users\1\Desktop\my-project\data'` imported from 'C:\Users\1\Desktop\get-data.ts'

找不到导入的模块，是因为没有在导入的文件中添加后缀名。

> `TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".ts"`

我收到这条报错的时候，命令行命令使用的是`ts-node ./myscripts.ts`，改用以下命令时，问题解决。

``` powershell
node --loader ts-node/esm ./my-script.ts
```

> `ReferenceError: fetch is not defined`

获取数据使用了`fetch`库，不是标准的Nodejs方法，需要下载`node-fetch`

``` powershell
yarn add node-fetch
```

## 写在最后

虽然程序员有很多交流问题的社区论坛，但跑了一圈后发现还是在`github`的`Issues`搜索问题更高速有效，其他地方很多无效甚至答非所问的回答。