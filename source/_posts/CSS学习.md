---
title: CSS学习
date: 2019-04-29 18:24:40
tags:
---

# CSS学习
## CSS样式
1. 内联式CSS样式：直接写在现有的HTML标签中
2. 嵌入式CSS样式：
	写在当前的文件中（把css样式代码写在&lt;style type="text/css">&lt;/style>标签之间）

3. 外部式CSS样式：写在单独的一个文件中
	使用&lt;link>标签将CSS样式文件链接到HTML文件内，
	如&lt;link href="base.css" rel="stylesheet" type="text/css" />

4. 优先级
	在内联式、嵌入式、外部式样式表中CSS是在相同权值的情况下，一般来说离被设置元素越近优先级级别越高。

## CSS选择器
每一条CSS声明（定义）由两部分组成，形式如下：
>选择器{样式;}
>在{}之前的部分就是"选择器"，
>"选择器"指明了{}中的"样式"作用于网页中的哪些元素。

### 标签选择器
标签选择题其实就是HTML代码中的标签，如&lt;HTML>等

### 类选择器
类选择器在CSS样式中是最常用到的。
>语法：.类选择器名称{CSS样式代码;}

注意：
>1、英文圆点开头
>2、其中类选择器名称可以任意起名

使用方法：
>第一步：使用合适的标签把要修饰的内容标记起来，如
>&lt;span>类选择器&lt;/span>
>第二步：使用class=“类选择器名称”为标签设置一个类，如
>&lt;span class="cls">类选择器&lt;/span>
>第三步：设置类选择器css样式，如
>.cls{color:red;}

### ID选择器
在很多方面，ID选择器都类似于类选择器，但也有一些重要的区别：
1、为标签设置id="id名称"，而不是class="类名称"。
2、id选择符的前面是#号，而不是英文圆点（.）。


### 类和ID选择器的区别
- ID选择器只能在文档中使用一次，而类选择器可以使用多次。
- 可以使用类选择器词列表方法为一个元素同时设置多个样式，但id选择器不可以。

### 子选择器
加入大于符号（>）用于选择指定标签元素的第一代元素。

### 包含（后代）选择器
加入空格，用于选择指定标签下的后辈元素。

### 包含选择器和子选择器的区别
子选择器只能选择直接后代元素，而包含选择器可以选择所有元素。

### 通用选择器
通用选择器是功能最强大的选择器，它使用一个(*)号指定，它的作用是匹配HTML中所有标签元素，如下面代码使用HTML中任意标签元素字体全部设置为红色：
```
* { clolr:red; }
```

### 伪类选择器
伪类选择器允许给HTML不存在的标签（标签的某种状态）设置样式，比如给HTML中的一个标签元素的鼠标滑过状态来设置字体颜色：
```
<html>
<head>
<style type="text/css">
a:hover{color:red;}
</style>
</head>

<body>
	<a>伪类选择器</a>
</body>
</html>

```

### 分组选择器
当你想为HTML中多个标签元素设置同一个样式时，可以使用分组选择符（,），如下代码：
```
h1,span{color:red;}     /\*使用分组选择符*/

h1{color:red;}
h1{color:red;}			/\*相当于两行代码*/
```

## CSS的继承、特殊性和层叠

### 继承
css的某些样式是具有继承性的。继承是一种规则，它允许样式不仅仅是用于某个特定HTML标签元素，而且应用于其后代，如某种颜色应用于p标签。但有一些css样式是不具有继承性的，如边框

### 特殊性
有时候我们为同一个元素设置了不同的css样式代码，那么元素会启用哪一个css样式？浏览器根据权值来判断使用哪种css样式，使用权值高的css样式。

权值的规则：
>标签的权值为1，
>类选择符的权值为10，
>ID选择符的权值为100，
>继承也有权值但很低。

### 层叠
层叠胡原始股在HTML文件中对于同一个元素可以有多个css样式存在，当有相同权重的样式存在时，会根据这些css样式的前后顺序来决定，处于最后面的css样式会被应用（离元素最近优先级越高）。

## css格式化排版
### 文字排版
- 字体
为网页中的文字设置字体，现在网页一般喜欢设置“微软雅黑”，如下代码
```
body{font-family:"Microsoft Yahei";}
或
body{font-family:"微软雅黑";}
```

- 字号、颜色
使用下面代码设置网页中文字的字号为12像素，并把字体颜色设置为#666（灰色）：
```
body{font-size:12px;color:#666}
```

- 粗体
使用下面代码设置文字以粗体样式显示出来：
```
p{font-weight:hold;}
```

- 斜体
使用下面代码设置文字以斜体样式显示出来：
```
p{font-style:italic;}
```


- 下划线
使用下面代码设置文字以下划线样式显示出来：
```
p{text-decoration:underline;}
```

- 删除线
使用下面代码设置文字以删除线样式显示出来：
```
p{text-decoration:line-through;}
```

### 段落排版
- 缩进
中文文字中的段前习惯空两个文字的空白，这个样式可以使用下面的代码来实现：
```
p{text-indent:2em;}
```

- 行间距
使用下面代码实现段落行间距为1.5倍：
```
p{text-height:1.5em;}
```

- 中文字间距、字母间距
网页排版中设置文字间隔或者字母间隔可以使用 letter-spacing 来实现，如下面代码：
```
h1{letter-sapce:50px;}
```
英文单词之间的间距：
```
h1{word-spacing:50px;}
```

- 对齐
块状元素设置雨中样式可以使用text-align样式代码，举例：
```
h1{text-align:center;}      /\*文字居中*/
h1{text-align:left;}		/\*文字居左*/
h1{text-align:right;}		/\*文字居右*/
```

## css盒模型
### 元素分类
在css中，HTML中的标签元素大体被分为三种不同的类型：块状元素、内联元素（行内元素）和内联块元素。

### 块级元素
在html中&lt;idv>、&lt;p>、&lt;h1>、&lt;form>、&lt;ul>、&lt;li>就是块级元素。设置`display:block`就是将元素显示为块级元素，使该元素具有块级元素的特点。

```
a{display:block;}
```

块级元素特点：
>1、每个块级元素都是从新的一行开始，并且其后的元素也另起一行。
>2、元素的高度、宽度、行高以及顶和底边距都可设置。
>3、元素宽度在不设置的情况下，是它本身父容器的100%，除非设定一个宽度。

### 内联元素
在htnl中，&lt;span>、&lt;a>、&lt;label>、&lt;strong>和&lt;em>就是典型的内联元素（行内元素）（inline）元素。块级元素也可以通过代码`display：inline`将元素设置为内联元素

```
div{display:inline;}
```

内联元素特点：
>1、和其他元素都在一行上；
>2、元素的高度、宽度及顶部和底部的边距不可设置；
>3、元素的宽度就是它包含的文字或照片的宽度，不可改变。

### 内联块状元素
内联块状元素就是同时具备内联元素和块状元素的特点，代码 `display:inline-block`就是将元素设置为内联块状元素。&lt;img>、&lt;input>标签就是这种内联块状标签。

inline-block元素特点：
>1、和其他元素都在一行上；
>2、元素的高度、宽度、行高以及顶部和底部边距都可设置。

### 盒模型--边框
盒子模型的边框就是围绕着内容及补白的线，这条线可以设置它的粗细、样式和颜色。

```
div{
border:2px  solid  red;
}
```

border代码的缩写形式分开写为：

```
div{
	border-width:2px;
	border-style:solid;
	border-color:red;
}
```

单独方向设置边框：

```
div{
border-bottom:1px solid red;
border-top:1px solid red;
border-right:1px solid red;
border-left:1px solid red;
}
```

### 盒模型--宽度和高度
css定义的宽（width）和高（height）指的是填充以里的内容的范围。
因此一个元素（盒子）实际宽度=左边界+左边框+左填充+内容宽度+右填充+右边框+右边界。

```
div{
	width:200px;
	padding:20px;
	border:1px solid red;
	margin:10px;
}
```

如上元素的实际长度为：10px+1px+20px+200px+20px+1px+10px=262px。

### 盒模型--填充
元素内容与边框之间是可以设置距离的，称之为“填充”。填充也分为上右下左（顺时针）。

```
div{padding:20px 10px 15px 30px;}
```

将上面代码分开写：

```
div{
	padding-top:20px;
	padding-right:10px;
	padding-bottom:15px;
	padding-left:30px;
}
```

- 如果上右下左的填充都为10px：

```
div{padding:10px;}
```

- 如果上下填充都为10px，左右填充都为20px：

```
div{padding:10px 20px;}
```

### 盒模型--边界
元素与其他元素之间的距离可以使用边界（margin）来设置，边界也分为上右下左（顺时针）。

```
div{margin:20px 10px 15px 30px;}
```

将上面代码分开写：
```
div{
	margin-top:20px;
	margin-right:10px;
	margin-bottom:15px;
	margin-left:30px;
}
```

- 如果上右下左的边界都为10px：

```
div{margin:10px;}
```

- 如果上下边界都为10px，左右边界都为20px：

```
div{mairgin:10px 20px;}
```

## css布局模型
css有三种基本的布局模型，流动模型（Flow）、层模型（Layer）和浮动模型（Float）。
### 流动模型
流动模型（Flow）是默认的网页布局模式。流动布局模型具有两个比较经典的特征：
1、块状元素都会在所处的包含元素内自上而下按顺序垂直延伸分布，因为在默认状态下，块状元素的宽度都为100%。实际上块状元素都会以行的形式占据位置。
2、在流动模型下，内联元素都会在所处的包含元素内从左到右水平分布显示。

### 浮动模型
可以用css定义为浮动，如div、p、table、img等元素都可以被定义为浮动。如下代码可以实现两个div元素一行显示。

```
#div{
	weith:200px;
	height:200px;
	border:2px red solid;
	float:left;
}
<div id="div1"></div>
<div id="div2"></div>
```

### 层模型
层布局模型就像是图像软件Photoshop中非常流行的图层编辑功能一样，每个图层能够精确定位操作。
层模型有三种形式：
1、绝对定位（position:absolute）
2、相对定位（position:relative）
3、固定定位（position:fixed）

### 绝对定位
如果想为元素设置层模型中的绝对定位，需要设置`position:absolute`，这条语句的作用是将元素从文档六中拖出来，然后使用left、right、top、bottom属性相对于最接近的一个具有定位属性的父包含块进行绝对定位，如果不存在这样的包含块，则相对于body元素，即相对于浏览器窗口。
如下边代码可以实现div元素相对于浏览器窗口向右移动100px，向下移动50px。

```
#div1{
	width:100px;
	height:200px;
	border:2px red solid;
	position:absolute;
	left:100px;
	top:50px;
}
<div id="div1"></div>
```

### 相对定位
如果想为元素设置层模型中的相对定位，需要设置`position:relative`，它通过left、right、top、bottom属性确定元素在正常文档流中的偏移位置，相对定位完成的过程首先是按static（float）方式生成一个元素，然后相对于以前的位置移动，移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保持不动。
如以下代码实现相对于以前位置向下移动50px，向右移动100px。

```
#div1{
	width:200px;
	height:200px;
	border:2px red solid;
	position:relative;
	left:100px;
	top:50px;
}

<div id="div1"></div>
```

### 固定定位
fixed：表示固定定位，与absolute定位类型相似，但它的相对移动的坐标是视图本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终定位于浏览器窗口内视图的某个位置，不会受文档流动影响。
以下代码可以实现相对于浏览器窗口视图向右移动100px，向下移动20px，并且拖动滚动条时位置固定不变。

```
#div1{
    width:200px;
    height:200px;
    border:2px red solid;
    position:fixed;
    left:100px;
    top:50px;
}
```

### Relative和absolute组合使用
使用`position:absolute`实现被设置元素相对于其它元素进行定位，需要使用`position:relative`并且遵守下面规范：

1. 参照定位的元素必须是相对定位元素的前辈元素：


```
<div id="box1"><!--参照定位的元素-->
    <div id="box2">相对参照元素进行定位</div><!--相对定位元素-->
</div>
```

2. 参照定位的元素必须加入`position:relative;`

```
#box1{
    width:200px;
    height:200px;
    position:relative;
}
````

3. 定位元素加入`position:absolute;`便可以用top、bottom、left、right来进行偏移定位了。

```
#box2{
    position:absolute;
    top:20px;
    left:30px;
}
```

这样box2就可以相对父元素box1定位了。