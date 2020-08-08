---
title: JS中的逻辑操作符
date: 2019-12-25 23:40:51
tags:
---

在JavaScript中，逻辑运算符可以操作ECMAScript中的**任意值**，同时也不强制返回boolean类型。

在js逻辑操作中，需要隐式的转换为boolean类型再计算。转换规则：

- 对象、非零Number、非空String  -> true
- 0、""、nullfalse、undefined、NAN -> false
- !!的作用是把一个其他类型的变量转换成bool类型

在||和&&逻辑操作中的短路原则：

`a && b`：左操作数为false，返回左操作数，否则返回右操作数。

`a || b`：左操作数为false时，返回右操作数，否则返回左操作数。

对于多个操作数的情况：

`a||b||c||d`：若结果为true则返回第一个true值，若结果为false则返回最后一个操作数。

`a&&b&&c&&d`：若结果为false则返回第一个false，若结果为true则返回最后一个操作数。

**使用场景：**

1、`||`操作符最常用的方式是用来从一组备选表达式中选出第一个真值表达式。

``` js
var max = max_width || perferences.max_width || 500;
```

2、判断某个元素是否存在时，`if(attr)`写成`if(!!attr)`更严谨。

3、对函数中的参数赋给默认值，`a = a || "defaultValue"`。

4、利用&&的短路特性有条件的执行代码。

- 在回调中，`callback && callback()`，先判断callback是否存在，存在才执行。
- 条件语句：`if (a == b) stop();`换成`(a == b) && stop();`。
- 判断某个对象存在再取值：`p && p.x`。