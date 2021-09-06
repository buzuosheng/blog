---
title: 使用Python处理Excel中的数据
date: 2020-11-12 22:24:08
tags: little tips
description:
categories: little tips
---

我们在办公日常用经常会接触到办公软件Excel，也会遇到大量数据迁移的问题，手动操作这么反人类的事情，俺们程序员肯定是不会干的。

Python这种编程语言，近几年在国内十分火热。很多程序员都多少会一点，用来写一些脚本。

今天使用Python来操作Excel。python操作Excel的库有很多，大概有`xlrd`、`xlwt`、`openpyxl`、`XlsxWriter`、`xlutils`、`pandas`等。这些库的操作对`xls和xlsx`的支持不同，有个只可以操作`xls`，有的只可以进行读操作。

我选用的库是`openpyxl`，支持对`xlsx`的读写操作。

## 安装

openpyxl的安装比较简单，可以只用`pip`直接安装。

``` shell
pip install openpyxl
```

这是在线安装的方式，离线安装的方式也有点反人类，不过也可以了解一下。

这种方式需要在[网站](https://www.lfd.uci.edu/~gohlke/pythonlibs/)先下载文件，网站地址是：https://www.lfd.uci.edu/~gohlke/pythonlibs/。

需要下载的文件有：`et_xmlfile-1.0.1-py2.py3-none-any.whl`、`jdcal-1.4-py2.py3-none-any.whl`、`openpyxl-2.6.0-py2.py3-none-any.whl`。

有vimium插件的可以使用`/`打开搜索，复制直接定位；没有的可以看看我之前介绍vimium插件的文章，也可以直接使用Chrome的快捷键`<C-f>`打开搜索栏。

下载了三个文件以后拷贝到python安装目录中的`scripts`目录下。

然后依次使用`pip install`命令安装即可成功。

## 介绍

`openpyxl`看名字就知道是一个开源项目，可以对`xlsx`文件进行读写操作。

在`openpyxl`中有三个对象，分别为`Workbook()`、`sheet`、`cell`。

一个Workbook有多个sheet，一个sheet有多个cell。

openpyxl有很多的API，我们只用到一部分。

## 获取对象

创建一个Excel的workbook对象。

``` python
import openpyxl

wb = openpyxl.Workbook()
```

如果编辑已有的excel文件，使用`load_workbook()`。

``` python
import os
import openpyxl

file_name = 'test.xlsx'
wb = openpyxl.load_workbook(file_name)
```

通过以上两种方式获得一个Workbook对象。一般默认是只有一个sheet。

获取当前正在工作的sheet。

``` python
ws = wb.active

# 创建一个sheet
ws1 = wb.create_sheet(sheet_name)

# 获取已有的sheet
ws2 = wb.get_sheet_by_name(sheet_name)

# 获取所有的sheet
sheets = wb.sheet_names
ws1 = sheets[0]
ws2 = sheets[1]
```

获取cell对象。

``` python
wc = ws.cell(row=1, column=1)
wc1 = ws['A1']
```

## 数据写入

使用sheet一行一行的加入数据；

``` python
row = [1, 2, 3, 4]
ws.append(row)
```

使用单元格写入数据：

``` python
ws['A1'] = 1
ws.cell(row=1, column=1).value = 1
```

一般可以通过for循环加上sheet对象的append()一行一行添加数据。

获取行或列批量操作：

``` python
# 按行操作
for row in ws.rows:
  pass

# 按列操作
for column in ws.columns:
  pass
```

使用单元格添加数据多为在以后的sheet中添加一列新的数据。例如

``` python
cells = ['A' + str(x) for x in range(1,11)]

# 可以配合获取sheet的最大列使用
cells = ['A' + str(x) for x in range(1, sheet.max_row)]
```

在做完所有的处理之后，保存文件：

``` python 
wb.save(file_name)
```

## 其他操作

设置sheet的标题：

``` pytohn
ws.title = 'title'
```

设置sheet颜色：

``` python
openpyxl.styles.PatternFill(fill_type='solid', fgColor="FFC125")
```

