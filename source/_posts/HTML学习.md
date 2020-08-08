---
title: HTML学习
date: 2019-04-26 00:55:41
tags:
---
# HTML学习

- HTML是网页内容的载体，用户浏览的信息。
- CSS样式是表现，如标题字体，颜色变化等。
- JavaScript是用来实现网页上的特效效果。
---

### HTML标签
- &lt;HTML>					根标签
>所有的网页标签都在里面

- &lt;head> 				文档头部
>用于定义文档的头部，是鄋头部元素的容器，有&lt;title>、&lt;scrip>、&lt;style>、&lt;link>、&lt;meta>等标签

- &lt;body> 				文档主体
> 之间的内容是网页的主要内容，如&lt;h1>、&lt;p>、&lt;a>、&lt;img>等网页内容标签，标签中的内容会在浏览器中显示出来

- &lt;hx>					标题标签（x为1-6，共六级标题）
- &lt;div>					块级标签
>可以划分出独立的逻辑部分

- &lt;p>					段落标签
- &lt;img src="图片路径">    图片标签
>1、scr：表示图像的位置；
>2、alt：指定图像的描述性文本，当图像不可见时（下载不成功时），可看到该属性指定的文本；
>3、提供在图像可见时对图像的描述(鼠标滑过图片时显示的文本)；
>4、图像可以是GIF，PNG、JPEG格式的图像文件。

- &lt;title>
>标签的内容会在浏览器的标题栏显示出来

- &lt;！--注释文字-->		 注释标签
- &lt;span>设置单独的样式
- &lt;q>引用文本
- &lt;br />换行显示文本
- &lt;code>单行代码
- &lt;pre>大量代码
>&lt;pre> 标签的主要作用:预格式化的文本。被包围在 pre 元素中的文本通常会保留空格和换行符。

-  &lt;a>标签
>使用&lt;a>标签可实现超链接，它在网页制作中可以说是无处不在，只要有链接的地方，就会有这个标签。
>&lt;a href="目标网址" title="鼠标滑过显示的文本">链接显示的文本&lt;/a>
>例如:
```
<a href="http//www.buzuosheng.com" title="点击进入不作声的博客">click here!</a>
 target="_blank" 在新建浏览器窗口打开链接
```

### 列表信息
```
<ul>
	<li>信息</li>
	<li>信息</li>
	<li>信息</li>
	<li>信息</li>
</ul>
```
在网页中显示的默认样式一般为：每个li前都自带一个圆点
```
<ol>
	<li>信息</li>
	<li>信息</li>
	<li>信息</li>
	<li>信息</li>
</ol>
```
在网页中显示的默认样式一般为：每个li前都自带一个序号，默认从1开始
### 网页上的表格
- &lt;table>表示整个表格
- &lt;tbody>
- &lt;tr>表格的一行
- &lt;td>表格的一列
- &lt;th>表格头部的第一个单元格
- &lt;caption>标题文本
```
<table>
    <caption>标题文本</caption>
    <tr>
        <td>…</td>
        <td>…</td>
        …
    </tr>
…
</table>
```

### 表单标签
- 网页使用HTML表单（form）与用户交互，表单可以把用户输入的数据传送到服务器端。

1. 语法：

```
<form  method="传送方式"  action="服务器文件">
```
>action：浏览者输入的数据被传送到的地方，比如一个PHP页面（save.php）。
>method：数据传送的方式（get/post）。

2. 例子：

```
<form    method="post"   action="save.php">
        <label for="username">用户名:</label>
        <input type="text" name="username" />
        <label for="pass">密码:</label>
        <input type="password" name="pass" />
</form>
```

### 输入框
当用户要在表单中键入字母、数字等内容时，就会用到文本输入框。文本框也可以转化为密码输入框。
1. 语法：
```
<form>
   <input type="text/password" name="名称" value="文本" />
</form>
```
>1、type：
>当type="text"时，输入框为文本输入框；
>当type="password"，输入框为密码输入框。
>2、name：为文本框命名
>3、value：为文本输入框设置默认值。（一般起提示作用）
2. 举例
```
<form>
  姓名：
  <input type="text" name="myName">
  <br/>
  密码：
  <input type="password" name="pass">
</form>
```

### 文本域
当用户需要在表单中输入大段文字时，需要用到文本输入域。
1. 语法：
```
<textarea  rows="行数" cols="列数">文本</textarea>
```

>1、cols：多行输入域的列数。
>2、rows：多行输入域的行数。
>3、&lt;text>&lt;/text>之间可以输入默认值。
2. 举例
```
<form  method="post" action="save.php">
        <label>联系我们</label>
        <textarea cols="50" rows="10" >在这里输入内容...</textarea>
</form>
```

### 选择框
html中有两种选择框，即单选框和复选框
1. 语法
```
<input type="radio/checkbox" value="值" name="名称" checked="checked"/>
```
>1、type：
>当type="radio"时，控件为单选框
>当type="checkbox"时，控件为复选框
>2、value：提交数据到服务器的值
>3、name：为控件命名，以备后台程序ASP、PHP使用（同一组的单选按钮，name取值一定要一致，这样同一组的单选按钮才可以起到单选的作用）
>4、checked：当设置checked="checked"时，该选项被默认选中

### 下拉列表框
1. 语法
```
<select>
      <option value="提交值">选项</option>
      <option value="提交值">选项</option>
      <option value="提交值">选项</option>
      <option value="提交值" selected="selected">选项</option>
    </select>
```

>value：向服务器提交的值，选项是在网页显示的值
>selected：设置selected="selected"属性，则该选项被默认选中
>在&lt;select>中添加multiple="multiple"可以实现多选

### 提交按钮
1. 语法：
```
<input type="submit" value="提交">
```
>type：只有当type值设置为submit时，按钮才有提交作用
>value：按钮上显示的文字

### 重置按钮
1. 语法：
```
<input type="reset" value="重置">
```
>type：只有当type值设置为reset时，按钮才有提交作用
>value：按钮上显示的文字

### form表单中的label标签
1. 语法
```
<label for="控件id名称">
```
注意：标签的for属性的值应当与相关控件的id属性值一定要相同。
2. 举例
```
<form>
  <label for="male">男</label>
  <input type="radio" name="gender" id="male" />
  <br />
  <label for="female">女</label>
  <input type="radio" name="gender" id="female" />
  <label for="email">输入你的邮箱地址</label>
  <input type="email" id="email" placeholder="Enter email">
</form>
```
