---
title: JavaScript学习（一）
date: 2019-05-03 01:01:01
tags:
---
# JavaScript学习（一）

## 学前准备



使用&lt;script>标签在HTML网页中插入JavaScript代码。&lt;script>标签要成对出现，并且JavaScript代码写在`&lt;script>&lt;/script>`之间。

`&lt;script type="text/javascript>`表示在&lt;script>&lt;/script>之间的是文本类型（text），javascript是为了告诉浏览器里面的文本是属于JavaScript语言。

我们可以把HTML文件和js代码分开，并单独创建一个JavaScript文件，其文件后缀名通常为 .js然后将JS代码直接写在JS文件中。



- 在HTML中添加代码：`<script src="script.js"></script>`。

- 在js文件中不需要&lt;script>标签，直接编写JavaScript代码即可。



我们可以将JavaScript代码放在html文件中任何位置，但是我们一般放在网页的head和body部分

1、放在&lt;head>部分
最常用的方式就是在页面中head部分放置&lt;script>元素，浏览器解析head部分就会执行这个代码，然后才解析页面的其余部分。

2、放在&lt;body>部分
JavaScript代码在网页读取到该语句的时候就会执行。

注意：JavaScript作为一种脚本语言可以放在HTML页面中任何位置，但是浏览器解析html是按先后顺序的，所以前面的script就先被执行，比如进项页面显示初始化的js就必须放在head里面。

每一句JavaScript代码格式：` 语句;`

单行注释在注释内容加符号“//”。

多行注释以“/\*”开始，以“\*/结束”。

定义变量使用关键字var，语法如下：

```
var 变量名
```

变量名可以任意取名，但要遵循命名规则：
1、变量必须使用字母、下划线或者美元符开始。
2、然后可以使用多个英文字母、数字、下划线、或者美元符组成。
3、不能使用JavaScript关键字与JavaScript保留字。

定义函数的基本语法：
```
function 函数名()
{
	函数代码;
}
```

## 常用互动方式
### JavaScript-输出内容
`docuoment.write()`可用于直接向HTML输出流写内容。

1. 输出内用""括起，直接输出""号内的内容。
```
<script type="text/javascript">
 document.write("I love javascript!");
</script>
```

2. 通过变量输出内容。
```
<script type="text/javascript">
  var mystr="hello";
  document.write(mystr);
</script>
```

3. 输出多项内容，内容之间用+号连接。
```
<script type="text/javascript">
  var mystr="hello";
  document.write(mystr+"I love JavaScript！");
</script>
```

4. 输出HTML标签，并起作用，标签用""括起来。
```
<script type="text/javascript">
  var mystr="hello";
  document.write(mystr);
  document.write("<br>");
  document.write("I love JavaScript！");
</script>
```

### JavaScript-警告
我们在访问网站的时候，有时候会突然弹出一个小窗口，上面写着一段提示信息文字。如果不点击“确定”，就不能对网页做任何操纵，这个小窗口就是使用alert实现的。
语法：
```
alert(字符串或代码);
```

如下面代码：
```
<script type="text/javascript">
	var mynum = 30;
	alert("hello");
	alert(mynum);
</script>
```

alert弹出消息对话框包含一个确定按钮
注意：
1、再点击对话框“确认”按钮前，不能进任何其它操作。
2、消息对话框通常可以用于调试程序。
3、alert输出内容可以是字符串或变量。

### JavaScript-确认
confirm消息对话框通常用于允许永华做选择的动作，如：”你确定吗？“等。弹出对话框（包括一个确定按钮和一个取消按钮）。

语法：
```
confirm(str);
```

参数说明：
>str：在消息对话框中要显示的文本
>返回值：Boolean值。当用户点击“确定”按钮时，返回true，当用户点击“取消”按钮时，返回false。

注意：在用户点击消息对话框前，不能进行任何其它操作。

### JavaScript-提问
prompt弹出消息对话框，通常用于询问一些需要与用户交互的信息。弹出消息对话框（包含一个确认按钮、取消按钮和一个文本输入框）。

语法：
```
prompt(str1, str2);
```
参数说明：
>str1：要显示在消息对话框中的文本，不可修改。
>str2：文本框中的内容，可以修改。

返回值：
>1、点击确认按钮，文本框中的内容将作为函数返回值。
>2、点击取消按钮，将返回null。

举例：
```
var myname=prompt("请输入你的姓名：");
if(myname!=null)
	{ alert("你好"+myname);}
else
	{ alert("你好 my friend");}
```
注意：在用户点击对话框的按钮前，不能进行任何其它操作。

### JavaScript-打开新窗口
open()方法可以查找一个已经存在或者新建的浏览器窗口。
语法：
```
window.open([URL],[窗口名称],[参数字符串])
```

参数说明：
>1、URL：可选参数，在窗口中要显示网页的网址或路径。如果省略这个参数，或者它的值是空字符串，那么窗口就不会显示任何文档。
>2、窗口名称：可选参数，被打开窗口的名称。
>>1.该名称有字母、数字和下划线字符组成。
>>2."_top"、"_blonk"、"_self"具有特殊意义的名称。"_blank"：在新窗口显示目标网页，"_self"：在当前窗口显示目标网页，"_top"：框架网页中在上部窗口显示目标网页。
>>3.相同name的窗口只能创建一个，要想创建多个窗口则name不能相同。
>>4.name不能包含空格。
>
>参数字符串：可选参数，设置窗口参数，各参数用逗号隔开。


### Script-关闭窗口
语法：
```
window.close();
```
或
```
<窗口对象>.close();
```

例如：关闭新建的窗口
```
<script type="text/javascript">
var mywin=window.open('http://www.buzuosheng.com');
mywin.close;
</script>
```
注意：上面代码在打开新窗口的同时，关闭该窗口，看不到被打开的窗口。

## DOM操作
### 认识DOM
文档对象模型DOM（document object model）定义访问和处理HTML文档的标准方法。DOM将HTML文档呈现为带有元素、属性和文本的树结构（节点树）。

HTML文档可以说由结点构成的集合，三种常见的DOM节点：
1、元素节点：&lt;html>、&lt;body>、&lt;p>等都是元素节点，即标签。
2、文本节点：向用户展示的内容，如&lt;li>...&lt;/li>中的JavaScript、DOM、CSS等文本。
3、属性节点：元素属性，如&lt;a>标签中的链接属性href="http://www.buzuosheng.com"。

### 通过ID获取元素
网页由标签将信息组织起来，而标签的id属性是唯一的，就像每人有一个身份证号一样，只要通过身份证号就可以找到相对应的人。在网页中，我们通过id先找到标签，然后再进行操作。

语法：
```
document.getElementById("id")
```

### innerHTML属性
innerHTML属性用于获取或替换HTML元素的内容。

语法：
```
Object.innerHTML
```
注意：
1、Object是获取的元素对象，如通过document.getElementById("ID")获取的元素。
2、注意书写，innerHTML区分大小写。

### 改变HTML样式
HTML DOM允许JavaScript改变HTML元素的样式。

语法：
```
Object.style.property=new style;
```

Object是获取的元素对象，如通过document.getElementById("id")获取的元素。

一些基本属性：
>backgroundColor&nbsp;&nbsp;&nbsp;&nbsp;设置元素的背景颜色
>height&nbsp;&nbsp;&nbsp;&nbsp;设置元素的高度
>width&nbsp;&nbsp;&nbsp;&nbsp;设置元素的宽度
>color&nbsp;&nbsp;&nbsp;&nbsp;设置文本的颜色
>font&nbsp;&nbsp;&nbsp;&nbsp;在一行设置所有的字体属性
>fontfamily&nbsp;&nbsp;&nbsp;&nbsp;设置元素的字体系列
>fontSize&nbsp;&nbsp;&nbsp;&nbsp;设置元素的字体大小

举例：改变&lt;p>元素的样式，将颜色改为红色，字号改为20，背景颜色改为蓝。
```
<p id="pcon">Hello World!</p>
<script>
	var mychar=decoment.getElementById("pcon");
	mychar.style.color="red";
	mychar.style.fontSize="20";
	mychar.style.backgroundColor="blue";
</script>
```

### 显示和隐藏
display属性可以设置网显示和隐藏效果。
语法：
```
Object.style.display = value
```

value取值：
1、none：此元素不会被显示，即隐藏。
2、block：此元素显示为块级元素，即显示。

### 控制类名
className属性设置或返回元素的class属性。
语法：
```
Object.className = classname
```

作用：
>1、获取元素的class属性。
>2、为网页内的某个元素指定一个css样式来更改该元素的外观。

