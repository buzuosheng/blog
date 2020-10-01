---
title: 善其器之git
date: 2020-07-30 20:40:41
tags: 工具类
categories: 学习从拥有一支好笔开始
---

# git版本控制工具

Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或大或小的项目。

Git与另一个版本控制系统有如下**区别**：

- Git是分布式的，SVN不是。
- Git把内容按元数据方式存储，而SVN是按文件。
- Git分支和SVN分支不同。
- Git没有一个全局的版本号，而SVN有。
- Git的内容完整性要优于SVN。

## 工作流程

首先要了解到Git分为几个区域，有**工作区、暂存区、版本库、远程仓库**。然后在后边会解释项目在各个区域移动。

单人工作比较简单，主要是重复的修改和提交，多用于个人项目。

- 初始化仓库；
- 现在就处于工作区，可以在项目中进行修改；
- 然后将更改提交到暂存区；
- 将更改从暂存区提交到本地仓库（版本库）；
- 再将代码推到远程仓库。

多人合作的话，每个人创建分支，在自己的分支上写代码，最后将分支合并。使用分支并不会影响开发主线的工作。

## 开发中经常使用的Git命令

1、**配置Git**

``` shell
git config
```

在使用Git之前。需要配置Git。主要配置的就是**用户名和邮箱**。然后再查看配置列表。

``` shell
git config --global user.name <name>
git config --global user.email <email>

git config -l
```

然后就可以开始使用了。

2、**初始化Git仓库**

``` shell
git init
```

该命令会在当前文件目录下初始化一个Git仓库并且生成一个`.git`目录。或者直接在github克隆一个仓库，使用如下命令。

``` shell
git clone <repo>
```

3、**添加到暂存区**

``` shell
git add <file>
```

将修改过的文件添加到暂存区，使用`git add . `或`git add -A`将所有更改的文件添加到暂存区。

**删除文件**可以使用`git rm`命令，只能删除工作区与暂存区的文件。如果想只删除暂存区的文件`git rm --cached <file>`。

**查看工作区的状态**使用`git status`命令，查看未添加的文件。

**查看暂存区中的文件**使用`git ls-files`命令。

4、**提交到本地仓库**

``` shell
git commit
```

在提交的时候需要添加提交信息`git commit -m <message>`，如此以来就知道每次提交做了什么更改。

在此时，已经使用过工作区、暂存区和本地仓库了。我们可以查看不同区的不同。

- 显示工作区与暂存区的不同：`git diff`
- 显示暂存区与本地仓库的不同：`git diff --cached`
- 显示三者的不同：`git diff HEAD`
- 仅显示改变的文件：`git diff --name-only`
- 显示两次提交的差异：`git diff <commit> <commit>`

5、**远程仓库**

``` shell
git remote
```

将代码提交到远程仓库之前需要建立连接。使用以下命令对远程仓库进行操作：

- **添加远程仓库**并命名为origin：`git remote add origin <git-repo>`
- **修改远程仓库**：`git remote set-url origin <git-repo`
- **删除远程仓库**：`git remote rm origin`
- **列出所有的远程仓库**：`git remote -v`

然后就可以推送到远程仓库了，其中origin是远程仓库，master是分支：

- **推送到远程仓库并建立追踪关系**：`git push -u origin master`
- **推送到远程仓库**：`git push origin master`

6、**分支**

**分支**绝对是Git中的核心概念。Git保存的不是文件的变化或差异，而是一系列不同时刻的快照。

**分支创建**

``` shell
git branch <branch>
```

分支的其他操作：

- **从远程仓库拉取文件**：`git pull origin master`

- **列出本地分支**：`git branch`
- **列出本地分支与追踪关系**：`git branch -vv`

- **列出远程分支**：`git branch -r`
- **列出所有分支**：`git branch -a`
- **删除已被合并的分支**：`git branch -d <branch>`
- **强制删除未被合并的分支**：`git branch -D <branch>`
- **更改分支名字**：`git branch -m <newbranch>`
- **设置追踪分支**：`git branch -u <upstream>`
- **切换分支**：`git checkout <branch>`
- **建立分支并切换工作区**：`git checkout -b <branch>`
- **切换到最近一次分支**：`git checkout -`
- **建立无任何提交历史的分支**：`git checkout --orphan <branch>`
- **合并develop分支到本分支**：`git merge develop`
- **合并最近切换分支**：`git merge - `

## 日志和标签

- **显示提交日志**：`git log`
- **以图表的形式显示提交日志**：`git log --graph --all --online --decorate`
- **列出所有标签并显示标签信息**：`git tag -ln`
- **在某个commit上添加一个标签**：`git tag v0.1 <commit>`
- **删除一个标签**：`git tag -d v0.1`

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)