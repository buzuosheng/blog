---
title: git提交代码规范
date: 2021-02-01 18:07:48
tags: ['github', 'git代码提交规范']
description: git commit命令提交代码规范
categories: github
---

在我们向github仓库提交代码时，`git commit`命令是不可缺少的。我们在commit时需要附带一些提交信息，否则将禁止提交。

我们一般都简短的写一下本次提交的内容，但是我们对于代码的提交是非常频繁的，时间一长，再回过头来看自己的提交记录，完全摸不着头脑。

所以git代码提交规范化是有大势所趋，将代码提交分类，让敲码生活更美好。

目前业界用的最多的就是Angular团队的规范，commit message主要格式如下：

``` 
<type>(<scope>):<subject>
<BlLANK_LINE>
<?body>
<BLANK_LINE>
<?footer>
```

主要有**header、body、fotter**三个部分，type是必须的，后两个可选。

## header

header只有一行，包括三个字段，**type(必选)**、**scope(可选)**、**subject(可选)**。

**type**字段用于说明commit的类型：

- feat：添加新功能
- fix：修补bug
- docs：修改文档
- style：修改样式不影响代码的运行逻辑
- refactor：代码重构
- test：增加测试
- chore：构建过程或者辅助工具变动

当type为feat或fix时，该commit应该出现在Change log，其他类型的commit不应出现在Change log中。

**scope**字段用于说明commit影响的范围，如视图层、控制层等，不是必选。

**subject**字段是commit目的的简要描述，格式：

- 以动词开头、使用第一人称现在时
- 第一个字母小写
- 句尾不加句号(.)

## body

body部分是本次commit的详细描述，可以分成多行。注意点：

- 第一人称现在时
- 应该描述本次代码变动的动机，以及与之前代码的对比

## footer

footer部分只用于两种情况：

- **不兼容变动**。如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。
- **关闭Issue**。如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

## 总结

Commitizen是一个规范git commit的工具，使用前需要使用命令`npm i -g commitizen`安装使用。

平时也可以不使用工具，在提交时使用类似`git commit -m 'type: subject'`的格式提交。