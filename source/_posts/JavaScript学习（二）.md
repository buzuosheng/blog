---
title: JavaScript学习（二）
date: 2019-05-06 18:21:12
tags:
---

# JavaScript学习（二）

## 操作符

### 比较操作符
```
var n1,n2,bool;
n1=1;
n2=2;
bool=n1>n2;//返回值为Boolean型
```

### 逻辑操作符
- 逻辑与操作符（&&）
`a>b && b>c`表示a大于b并且b大于c。

- 逻辑或操作符（||）
`a>b && b>c`表示a大于b或b大于c。

- 逻辑非操作符（!）
`c=!(a>b)`

### 操作符优先级
操作符之间的优先级：
算术操作符>比较操作符>逻辑操作符>赋值操作符

## 数组
数组是一个值的集合，每个值都有一个索引号，从0开始，每个索引都有一个相应的值，根据需要添加更多数值。

### 创建数组
使用数组之前要先创建一个数组，并将数组赋值个一个变量。
创建数组语法：
```
var myarray =new Array(5);//5表示数组中存储5个数据
```

注意：

1. 创建的新数组是空数组，没有值，如果输出则显示undefined。
2. 虽然创建数组时指定的长度，但实际上数组都是变长的。


### 数组赋值
数组赋值有多种方法：
1、创建数组后挨个赋值，
```
var myarray = new Array();
myarray[0]=10;
myarray[1]=20;
```

2、创建数组同时赋值，
```
var myarray = new Array(10,20);
```

3、直接输入一个数组（称“字面量数组”），
```
var myarray = [10,20];
```

注：数组存储的数据可以是任何类型，如数字，字符，布尔值等。

### 数组属性length
如果我们想知道数组的大小，只需要引用数组的一个属性length。length属性表示数组的长度，即数组中元素的个数。
语法：
```
myarray.length;
```

注：数组的上下限分别为0和length-1。

JavaScript数组的length属性是可变的。
```
arr.length = 10;//将数组的长度变为10
```

数组的长度随着元素的增加长度也会改变。
```
var arr = [1,2,3,4,5];
document.write(arr.length);
arr[15] = 34;
alert(arr.length);
```

### 二维数组
二维数组的表示：`myarry[][]`

1、二维数组的定义方法一：
```
var myarr = new Array();	//先声明一维
for(var i=0;i<2;i++){		//一维长度为2
	myarr[i] = new Array();//再声明二位
	for(var j=0;j<3;j++){	//二维长度为3
	marry[i][j]=i+j;		//赋值，每个元素的值为i+j
	}
}
```

2、二维数组的定义方法二：
```
var Myarr = [[0,1,2],[1,2,3]];
```

2、赋值
```
Myarr[0][1] = 5;	//0表示行，1表示列
```

## 流程控制语句

### 判断语句
if语句是基于条件城里才执行相应代码时使用的语句。
语法：
```
if(判断条件){
	条件成立时的执行语句
}else{
	条件不成立时的执行语句
}
```

### switch语句
switch语法：
```
switch(表达式){
	case值1:
		执行代码块1
		break;
	case值2:
		执行代码块2
		break;
	...
	case值n:
		执行代码块n
		break;
	default;
		不符合上述所有条件时的执行代码块
}
```
说明：
>switch必须赋初值，值与每个case值相匹配。执行完该case后的所有语句后用break语句阻止运行下一个case。


### for循环
当满足判断条件后，重复执行循环语句。
for语句结构：
```
for(初始化变量;循环条件;循环迭代)
{
	循环语句;
}
```

### while循环
执行一段代码，直到不满足判断条件。
while语句结构：
```
while(判断条件)
{
	循环语句
}
```

### do...while循环
do while结构的基本原理和while结构是基本相同的，但是它保证循环体至少被执行一次。因为它是先执行代码，后判断条件，如果条件为真，继续循环。

do...while语句结构：
```
do
{
	循环语句
}
while(判断条件)
```

### 退出循环break
格式：
```
for(初始条件；判断条件；循环后条件值更新)
{
	if(特殊条件)
	{break;}
	循环代码
}
```

出现特殊情况的时候循环就会立即结束。

### 继续循环continue
continue的作用仅仅是跳过本次循环，后续的循环则不会受影响。
语句结构：
```
for(初始条件;判断条件;循环后值更新)
{
	if(特殊情况)
	{continue;}
	循环代码
}
```


## 函数
函数的作用是可以写一次代码，然后反复的重用这段代码。

function是定义函数的关键字，“函数名”是为函数取的名字，“函数体”替换为完成特定功能的代码。

函数定义好后是不能自动执行的，需要调用它，直接在需要的位置写函数名。

### 函数调用
1、在&lt;script>标签内调用
```
<script type="text/javascript">
	function add2()
	{
		sum = 1 + 1;
		alert(sum);
	}
	add2();//调用函数，直接写函数名。
</script>
```

2、在HTML文件中调用，如通过点击按钮后调用定义好的函数
```
<html>
<head>
<script type="text/javascript">
   function add2()
   {
         sum = 5 + 6;
         alert(sum);
   }
</script>
</head>
<body>
<form>
<input type="button" value="click it" onclick="add2()">  //按钮,onclick点击事件，直接写函数名
</form>
</body>
</html>
```


```
function 函数名(参数)
{
	函数代码
}
```

参数可以设置多个，根据需要增减参数个数，参数之间用逗号隔开。
```
function 函数名()
{
	函数代码
	return sum;
}
```

## 事件
JavaScript创建动态页面。事件是可以被JavaScript侦测到的行为，网页中的每个元素都可以产生某些触发JavaScript函数或程序的事件。

主要事件表：

| 事件   		| 说明     |
|:-------:   | :-----:   |
| onclick    | 鼠标单击事件|
| onmouseover | 鼠标经过事件|
| onmouseout  | 鼠标移开事件|
|onchange	  |文本框内容改变事件|
|onselect	|文本框内容被选中事件|
|onfocus	|光标聚集|
|onblur		|光标离开|
|onload		|网页导入|
|onunload	|关闭网页|

- 鼠标单击事件(onclick)
onclick是鼠标单击事件，当在网页上单击鼠标时，就会发生该事件，同时onclick事件调用的程序块就会被执行，通常与按钮一起使用。

- 鼠标经过事件(onmouseover)
鼠标经过事件：当鼠标移动到一个对象上时，该对象就触发onmouseover事件，并执行onmouseover事件调用的程序。

- 鼠标移开事件(onmouseout)
鼠标移开事件，当鼠标移开当前对象时，执行onmouseout调用的程序。

- 光标聚焦事件(onfocus)
当网页中的对象获得焦点时，执行onfocus调用的程序。如当光标移动到文本框内时，即焦点在文本库内，触发onfocus事件。

- 失焦事件(onblur)
onblur事件和onfocus事件是相对事件，当光标离开当前获得聚焦对象的时候，触发onblur事件，同时执行被调用的程序。

- 内容选中事件(onselect)
选中事件，当文本框或文本域中的文字被选中时，触发onselect事件，同时调用的程序就会被执行。

- 文本框内容改变事件(onchange)
当文本框中的内容被改变后，就会触发onchange事件，并执行被调用的程序。

- 加载事件(onload)
事件会在页面加载完成后立即发生，同时执行被调用的程序。
注意：加载页面时，触发onload事件，事件卸载&lt;body>标签内。

- 卸载事件(onunload)
当用户退出页面时（页面关闭、页面刷新等），触发onUnload事件，同时执行被调用的程序。
注意：不同浏览器对onUnload事件支持不同。
```
window.onunload = onunload_message();
	function onunload_message(){
        alert("您确定离开该网页吗？");
    }
```

