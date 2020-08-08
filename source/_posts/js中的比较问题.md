---
title: js中的比较问题
date: 2020-03-01 15:48:05
tags:
---

## ==和===有什么区别

​	在使用`==`时，如果左右两边的类型不同，就会进行转换，然后再比较值。在使用`===`时，直接比较左右两边的类型和值，如果类型不同，结果直接为false。

## 原始类型的转换和比较

`Number()`转换为数字，`String()`转换为字符串，`Boolean`转换为布尔值。

### number类型转换

``` js
//number转string
String(0)	//'0'
String(1)	//'1'

//number转Boolean
Boolean(0)	//false
Boolean(1)	//true
```

- number转为string类型时，直接加上引号即可。
- number转为Boolean类型时，除0为false外，其他都为true。

### string类型转换

``` js
//转为number类型
Number('0')		//0
Number('a')		//NaN
Number('\t')	//0

//转为Boolean类型
Boolean('0')	//true
Boolean('')		//false
Boolean('/tr')	//true
```

- string转为number类型时，去掉引号为数字则结果为相应的数字，否则返回NaN。`\t、\r、\n`转为number类型时结果为0。
- string转为Boolean类型时，除空字符串为false外，其他都为true。

## Boolean类型转换

``` js
//转为number类型
Number(true)	//1
Number(false)	//0

//转为string类型
String(true)	//'true'
String(fasel)	//'false'
```

- Boolean只有true和false两个值，转为number分别为1和0。

## null类型转换

``` js
Number(null)	//0
String(null)	//'null'
Boolean(null)	//false
```

## undefined类型转换

``` js
Number(undefined)	//NaN
String(undefined)	//'undefined'
Boolean(undefined)	//false
```

## 常见的类型转换的坑

``` js
0 == ''			//true
0 == '0'		//true

2 == true		//false
2 == false		//false

false == 'false'	//false
false == ''			//true

false == undefined	//false
false == null		//false
null == undefined	//true

'\t\r\n' == 0 		//true	

[] == [] 			//false
[] == ![]			//true

{} == {}			//
```

- 并不能因为`Boolean(2)`结果为`true`，得出`2 == true`为true的结果。

## 引用类型比较

任意两个对象都不相同。