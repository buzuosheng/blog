---
title: JavaScript学习（三）
date: 2019-05-13 23:56:22
tags: JavaScript学习
---

# JavaScript学习（三）

## JavaScript内置对象
JavaScript中的所有事物都是对象，如：字符串、数值、数值、函数等，每个对象带有属性和方法。

对象的属性：反映该对象某些特定的性质。如：字符串的长度、图像的长宽等。
对象的方法：能在对象上执行的操作。如：表单的提交，时间的获取等。

JavaScript提供多个内建对象，比如String、Date、Array等，使用对象前需要先定义。
访问对象属性的语法：
```
objectName.propertyName
```

访问对象方法的语法：
```
objectName.methodName()
```

### Date日期对象
日期对象可以存储任意一个日期，并且可以精确到毫秒数。

定义一个时间对象：`var myDate = new Date()`
使myDate成为时期对象，并且已有初始值：当前电脑系统时间

Date对象中处理时间和日期的常用方法：

|方法名称|功能描述|
|:--:|:--:|
|get/setDate()|返回/设置日期|
|get/setFullYear()|返回/设置年份，用四位数表示|
|get/setYear()|返回/设置年份|
|get/setMonth()|返回/设置年份 0~11表示一月到十二月|
|get/setHours()|返回/设置小时，24小时制|
|get/setMInutes()|返回/设置分钟数|
|get/setSeconds()|返回/设置秒钟数|
|get/setTime()|返回/设置年份|

- 返回星期方法
getDay()返回星期，返回的是0~6数字，0表示星期天。如果想要返回对应的星期，通过数组完成。

```
<script type="text/javascript">
  var mydate = new Date();
  var weekday = ["星期天","星期一","星期二","星期三","星期四","星期五","星期六" ];
  docoument.write("今天是：" + weekday[mydate.getDate()]);
</script>
```

### String字符串对象
定义字符串的方法就是直接赋值。定义字符串后我们就可以访问它的属性和方法。

访问字符串对象的属性length：
```
var mystr = "I love JavaScript!";
var myl = mystr.length;
```

访问字符串对象的方法：
```
var mystr = "I love JavaSctript!";
var newstr =mystr.toUpperCase();
```

1. 返回指定位置的字符
语法：`stringObject.charAt(index)`

注意：

- index必需。表示字符串中某个位置的数字，及字符在字符串中的下标。
- 字符串中第一个字符的下标是0，最后一个字符的下标为（string.length-1）。
- 如果参数index不在0与string.length-1之间，该方法将返回一个空字符串。
- 一个空格也算一个字符。

2. 返回指定的字符串首次出现的位置
indexOf()方法可以返回某个指定的字符串值在字符串中首次出现的位置。
语法：`stringObject.indexOf(substring, startPos)`
注意：

- substring必须，是需要检索的字符串值。
- startpos可选，规定在字符串中开始检索的位置，取值范围是0到stringObject.length-1。如果省略该参数，则从字符串的首字符开始检索。
- indexOf()方法区分大小写。
- 如果要检索的字符串值没有出现，则该方法返回-1。

3. 字符串分割split()

split()方法将字符串分割为字符串数组，并返回次数组。
语法：`stringObject.split(sepatator,limit)`
注意：

- separator必需，从参数指定的地方分割stringObject。
- limit可选，分割的次数。如不设置参数则不限制次数。
- 如果把空字符("")用作separator，那么stringObject中的每个字符之间都会被分割。

4. 提取字符串substring()
substring()方法用于提取字符串中介于两个指定小标的字符。
语法：`tringObject.substring(startPos,stopPos)`
注意：

- startPos必须，一个非负的整数，开始位置。
- stopPos可选，一个非负的整数，结束位置。如果省略则会一直提取到字符串对象的结尾。
- 返回的内容是从start开始（包含start位置的字符）到stop-1处的所有字符，其长度为stop-start。
- 如果start与stop相等，那么该方法返回的就是一个空串，即长度为0的字符串。
- 如果start比stop大，那么该方法在提取子串之前会先交换这两个参数。

5. 提取指定数目的字符串substr()

substr()方法从字符串中提取从startPos位置开始的指定数目的字符串。
语法：`stringObject.substr(startPos,length)`
注意：

- length可选，字符串的长度，如果省略则会一直提取到字符串对象的结尾。
- 如果参数startPos是负数，从字符串的尾部开始算起的位置，也就是说-1指字符串中最后一个字符，-2指字符串中倒数第二个字符，以此类推。
- 如果startPos为负数且绝对值大于字符串长度，startPos为0；

### Math对象
Math对象，提供对数据的数学计算。
Math对象方法：

|方法|描述|
|:--:|:--:|
|abs(x)|返回数的绝对值|
|ceil(x)|对数进行上舍入|
|floor(x)|对数进行下舍入|
|max(x,y)|返回x和y中的最高值|
|min(x,y)|返回x和y中的最低值|
|pow(x,y)|返回x的y次幂|
|random()|返回0~1之间的随机数|
|round(x)|把数四舍五入为最接近的整数|
|sqt(x)|返回数的平方根|
|toSource()|返回该对象的源代码|
|valueOf()|返回Math对象的原始值|

- 向上取整ceil()
语法：`Math.ceil(x)`

- 向下取整floor()
语法：`Math.floor(x)`

- 四舍五入round()
语法：`Math.round(x)`

- 随机数random()
random()方法可返回一个0~1之间的随机数，每次返回的值都不一样。
语法：`Math.random()`

### Array数组对象

数组方法：

|方法|描述|
|:--:|:--:|
|concat()|连接两个或更多个数组，并返回结果|
|join()|把数组的所有元素放入一个字符串，元素通过指定的分隔符进行分割|
|pop()|删除并返回数组的最后一个元素|
|push()|向数组的末尾添加一个或多个元素，并返回新的长度|
|reverse()|颠倒数组中元素的顺序|
|shift()|删除并返回数组的第一个元素|
|slice()|从某个已有的数组返回选定的元素|
|sort()|对数组的元素进行排序|
|splice()|删除元素，并向数组添加新元素|
|toSource()|返回该对象的源代码|
|toString()|把数组转换为字符串，并返回结果|
|unshift()|向数组的开头添加一个或多个元素，并返回新的长度|
|valueOf()|返回数组对象的原始值|

- 数组连接concat()

concat()方法用于连接两个或多个数组，此方法返回一个新数组，不改变原来的数组。



语法：`arrayOject.concat(array1,array2,...,arrayN)`



- 指定分隔符连接数组元素join()

join()方法用于把数组中的所有元素放入一个字符串，元素是通过指定的分割符进行分割的。



语法：`arrayObject.join(分隔符)`

如果省略分隔符则用逗号作为分隔符。

该方法返回一个字符串，不影响数组原本的内容。



- 颠倒数组元素顺序reverse()

语法：`arrayObject.reverse()`



注意：该方法会改变原来的数组，而不会创建新的数组。



- 选定元素slice()



slice()方法可从已有的数组中返回选定的元素。



语法：`arrayObject.slice(start,end)`

注意：

1、start必需，规定从何处开始选取。如果是负数，则从数组的尾部开始算起的位置，也就是说-1指最后一个元素，-2指倒数第二个元素，以此类推。

2、end可选，规定从何处结束选取。如果没有设置参数，则切分的数组包括从start到结束的所有元素。

3、返回一个新的数组，包含从start到end（不包含该元素）的arrayObject中的元素。

4、该方法不会修改数组，而是返回一个子数组。



- 数组排序sort()

sort()方法使数组中的元素按照一定的顺序排序。



语法：`arrayObject.sort(方法函数)`

注意：

1、如果不指定方法函数，则按unicode码顺序排列。



举例：

```

<script type="text/javascript">

  function sortNum(a,b){

  return a - b;//升序

}

  var myarr o= new [80,16,50,6,100,1];

  document.write(myarr.sort(sortNum));

</script>

```



## 浏览器对象
window对象是BOM的核心，window对象指当前的浏览器窗口。

### window对象方法：

|方法|描述|
|:--:|:--:|
|alert()|显示一段消息和一个确认按钮的警告框|
|prompt()|显示可提示用户输入的对话框|
|confirm()|显示带有一段消息以及确认按钮和取消按钮的对话框|
|open()|打开一个新的浏览器窗口或查找一个已命名的窗口|
|close()|关闭浏览器窗口|
|print()|打印当前窗口的内容|
|focus()|把键盘焦点给予一个窗口|
|blur()|把键盘焦点从顶层窗口移开|
|moveBy()|可相对窗口的当前坐标把它移动指定的像素|
|moveTo()|把窗口的左上角移动到一个指定的坐标|
|resizeBy()|按照指定的像素调整窗口大小|
|resizeTo()|把窗口的大小调整到指定的宽度和高度|
|scrollBy()|按照指定的像素值来滚动内容|
|scrollTo()|把内容滚动到指定的坐标|
|setInterval()|每隔指定的时间执行代码|
|setTimeout()|在指定的延迟时间之后执行代码|
|clearInterval()|取消setInterval()的设置|
|clearTimeout()|去掉setTimeout()的设置|

### JavaScript计时器
在JavaScript中，我们可以在设定的时间间隔之后来执行代码，而不是在函数被调用后立即执行。

计时器类型：
一次性计时器：仅在指定的延迟时间之后触发一次。
间隔性触发计时器：每隔一定的时间间隔就触发一次。

- 计时器setInterval()

在执行时，从载入页面后每隔指定时间执行代码。
语法：`setInterval(代码,交互时间);`

参数说明：
1、代码：要调用的函数或要执行的代码串。
2、交互时间：周期性执行或调用表达式之间的时间间隔，以毫秒计。
返回值：一个可以传递给clearInterval()从而取消对“代码”的周期性执行的值。

- 取消计时器clearInterval()

clearInterval()方法可取消由setInterval()设置的交互时间。
语法：`clearInterval(id_of_setInterval)`

参数说明：id_of_setInterval：由setInterval()返回的id值。

- 计时器setTimeout()
setTimeout()计时器，在载入后延迟指定时间后，去执行一次表达式，仅执行一次。

语法：`setTimeout(代码,延迟时间);`

- 取消计时器clearTimeout()
setTimeout()和clearTimeout()一起使用，停用计时器。

语法：`clearTimeout(id_of_setTimeout)`

参数说明：
id_of_setTimeout是由setTimeout()返回的ID值。该值标识要取消的延迟执行代码块。
无限循环：在定义函数体里调用该函数。

### History对象
history对象记录了用户曾经浏览过的画面（URL），并可以实现浏览器前进与后退相似导航的功能。

注意：从窗口被打开的那一刻开始记录，每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。

语法：`window.history.[属性|方法] //window可以省略`

History对象属性：
length：返回浏览器历史列表中的URL数量。

History对象方法：

|方法|描述|
|:--:|:--:|
|back()|加载history列表中的前一个URL|
|forword()|加载history列表中的下一个URL|
|go()|加载history列表中的某个具体的页面|

- 返回前一个浏览的页面
`window.hitory.back();`

- 返回下一个浏览的页面
`window.history.forward;`

- 返回浏览历史中的其他页面
`window.history.go(number);`

### Location对象
location用于获取或设置窗体的URL，并且可以用于解析URL。
语法：`location.[属性|方法]`

location对象属性：

|属性|描述|
|:--:|:--:|
|hash|设置或返回从#号开始的URL（锚）|
|host|设置或返回主机名和当前URL的端口号|
|hostname|设置或返回当前URL的主机名|
|href|设置或返回完整的URL|
|pathname|设置或返回当前URL的路径部分|
|port|设置或返回当前URL的端口号|
|protocol|设置或返回当前URL的协议|
|search|设置或返回从问号开始的URL（查询部分）|

location对象方法：

|方法|描述|
|:--:|:--:|
|assign()|加载新的文档|
|reload()|重新加载当前文档|
|replac()|用新的文档替换当前文档|

### Navigator对象
Navigator对象包含有关浏览器的信息，通常用于检测浏览器与操作系统的版本。

对象属性：

|属性|描述|
|:--:|:--:|
|appCodeName|浏览器代码名的字符串表示|
|appName|返回浏览器的名称|
|appVersions|返回浏览器的平台和版本信息|
|platform|返回运行浏览器的操作系统版本|
|userAgent|返回由客户机发送服务器的user-agent头部的值|

### screen对象
screen对象用于获取用户的屏幕信息。
语法：`window.screen.属性`

screen对象属性：

|属性|描述|
|:--:|:--:|
|availHeight|窗口可以使用的屏幕高度，单位像素|
|availWidth|窗口可以使用的屏幕宽度，单位像素|
|colorDepth|用户浏览器表示的颜色位数，通常为32位（每像素的位数）|
|pixelDepth|用户浏览器表示的颜色位数，通常为32位（IE不支持此属性）|
|height|屏幕的高度，单位像素|
|width|屏幕的宽度，单位像素|

## DOM对象
文档对象模型DOM（Document Object Model）定义访问和处理HTML文档的标准方法。DOM将HTML文档呈现为带有元素、属性和文本的树结构（节点数）。

### getElementsByName()方法
返回电邮指定名称的节点对象的集合。
语法：`document.getElementsByName(name)`

注意：

1、与getElementById()方法不同的是，通过元素的name属性查询元素，而不是id属性。因为文档中的name属性可能不唯一，所有getElementsByName()方法返回的是元素的数组，而不是一个元素。

2、和数组类似也有length属性，可以和访问数组一样的方法来访问，从0开始。

### getElementsByTagName()方法
返回带有指定标签名的节点对象的集合。返回元素的顺序是它们在文档中的顺序。
语法：`document.getElementByTagName(Tagname)`

说明：
1、Tagname是标签的名称，如p,a,img等标签名。
2、和数组类似也有length属性，可以访问数组一样的方法来访问，所以从0开始。

### getElementById,getElementByName,getElementByTagName的区别

- ID是唯一的，所以通过getElementById获取的是指定的一个对象。
- Name是标签的名字，可以重复。所以通过getElementByName获取的是相同名字的对象的集合。
- TagName是某类对象。通过getElementByTagName获取的是相同类的对象的集合。

### getAttribute()方法
通过元素节点的属性名称获得属性的值。
语法：`elementNode.getAttribute(name)`

说明：
1、elementNode：使用getElementById()、getElementsByTagName()等方法获取到的元素节点。
2、name：想要查询的元素节点的属性名字

### setAttribute()方法
setAttribute()方法增加一个指定名称和值的新属性，或者把一个现有的属性设定为指定的值。
语法：`elementNode.setAttribute(name,value)`

注意：
1、把指定的属性设置为指定的值。如果存在具有指定名称的属性，该方法将创建一个新属性。
2、类似于getAttribute()方法，setAttribute()方法只能通过元素节点对象调用的函数。

### 节点属性
在文档对象模型（DOM）中，每个节点都是一个对象。DOM节点有三个重要的属性：
1、nodeName：节点的名称
2、nodeValue：节点的值
3、nodeType：节点的类型

一、nodeName属性：节点的名称，是只读的。
1.元素节点的nodeName与标签名相同
2.属性节点的nodeName事属性的名称
3.文本节点的nodeName永远是#text
4.文档节点的nodeName永远是#document

二、nodeValue属性：节点的值
1.元素节点的nodeValue是undefined或null
2.文本节点的nodeValue是文本本身
3.属性节点的nodeValue是属性的值

三、nodeType属性：节点的类型，是只读的。以为常用的几种节点类型：

|元素类型|节点类型|
|:--:|:--:|
|元素|1|
|属性|2|
|文本|3|
|注释|8|
|文档|9|

### 访问子节点childNodes
访问选定元素节点下的所有子节点的列表，返回的值可以看作是一个数组，具有length属性。

语法：`elementNode.childNodes`
注意：
如果选定的节点没有子节点，则该属性返回不包含节点的NodeList。

### 访问子节点的第一项和最后项
一、`firstChild`属性返回‘childNodes’数组的第一个子节点。如果选定的节点没有子节点，则该属性返回NULL。
语法：`node.firstChild`
说明：与elementNode.childNodes[0]是同样效果。

二、`lastChild`属性返回‘childNodes’数组的最后一个子节点。如果选定的节点没有子节点，则该属性返回NULL。
语法：`node.lastChild`
说明：与elementNode.childNodes[elementNode.childNodes.length-1]是同样的效果。

### 访问父节点
获取指定节点的父节点，语法：`elementNode.parentNode`

### 访问兄弟节点
1.nextSibling属性可返回某个节点之后紧跟的节点（处于同一树层级中）。
语法：`nodeObject.nextSibling`

2.previousSibling属性可返回某个节点之前紧跟的节点（处于同一树层级中）。
语法：`nodeObject.previousSibling`

如果不存在这样的子节点，则该属性返回null。

### 插入节点appendChild()
在指定节点的最后一个子节点列表之后添加一个新的子节点。
语法：`appendChild(newnode)`

参数：newnode是指定追加的结点。

### 插入节点insertBefore()
insertBefore()方法可在已有的子节点前插入一个新的子节点。
语法：`insertBefore(newnode,node);

参数：
newnode：要插入的新节点。
node：指定此节点前插入节点。

### 删除节点removeChild()
removeChild()方法从子节点列表中删除某个节点。如果删除成功，此方法可返回被删除的节点，如果失败则返回NULL。

语法：`nodeObject.removeChild(node)`

### 替换元素节点replaceChild()
replaceChild实现子节点（对象）的替换。返回被替换对象的引用。

语法：`node.replaceChil(newnode,oldnode)`

### 创建元素节点createElement()
createElement()方法可创建元素节点。此方法返回一个Element对象。

语法：`document.createElement(tagName);`
参数：tagName字符串值，用来指明创建元素的类型。

要与appendChild()或insertBefore()方法联合使用，将元素显示在页面中。

### 创建文本节点createTextNode()
createTextNode()方法创建新的文本节点，返回新创建的Text节点。

语法：`document.createTextNode(data)`
参数：data：字符串值，可规定此节点的文本。

