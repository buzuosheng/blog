---
title: react学习
date: 2019-07-15 00:42:39
tags:
---
# React核心概念

## JSX简介
JSX是一个JavaScript的语法扩展。因为JSX在语法上更简洁JavaScript而不是HTML，所以React DOM使用cameCase来定义属性的名称，而不是用HTML属性名称的命名约定。

## 元素渲染
元素是构成React应用的最小砖块，描述了你想在屏幕上看到的内容。与浏览器的DOM元素不同，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与react保持一致。

### 将一个元素渲染为DOM
想要将一个React元素渲染到根DOM节点中，只需把它们一起传入ReactDOM.render()

### 更新已渲染的元素

React元素是不可变对象。一旦被创建，你将无法更改它的子元素或者属性。一个元素就像电影的单帧，它代表了某个特定时刻的UI。

根据我们已有的知识，更新UI唯一的方式是创建一个全新的元素，并将其传入ReactDOM.render()。

```
function tick(){
 const element = (
 <div>
   <h1>Hello, world</h1>
   <h2>It is{new Date().toLcaleTimeString()}.</h2>
   </div>
 );

 ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

这个例子中setInterval函数每秒都调用ReactDOM.render()。

### React只更新它需要更新的部分
React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会哦进行必要的更新来使DOM达到预期的状态。

## 组件&Props
组件允许将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。

### 函数组件与class组件
定义组件最简单的方式就是编写JavaScript函数：

```
function Welcome(props){
 render(){
   return <h1>Hello,{props.name}</h1>;
 }
}

```

该函数是一个有效的React组件，因为它接收唯一带有数据的“props”（代表属性）对象并返回一个React元素。这类组件被称为“函数组件”，因为它本质上就是JavaScript函数。



### 渲染组件
React元素也可以是用户自定义的组件：`const element = <Welcome name = "Sara" />;`

当React元素为用户自定义组件时，它会将JSX所接收的属性(attributes)转换为单个对象传递给组件，这个对象被称之为“props”。

### 组合组件
组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。按钮，表单，对话框，甚至整个屏幕的内容：在React应用程序中，这些通常都会以组件的形式表示。

### 提取组件
将组件拆分为更小的组件。如果UI中有一部分被多次使用（BUtton，Panel，Avatar），或者组件本身就够复杂（APP，FeedStory，Comment）那么它就是一个可复用组件的候选项。

### Props的只读性
组件无论是使用函数声明还是通过class声明，都决不能修改自身的props。**所有的React组件都必须像纯函数那样保护它们的props不被改变。**

## State & 生命周期

一、开始封装时钟的外观：
```
function clock(props){
  return(
  <div>
    <h1>Hello, world!</h1>
	<h2>It is {props.date.toLocaleTimeString()}.</h2>
  </div>
  )
}

function tick(){
  reactDOM.render(
  <Clock date = {new Date()} />
  document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

然而Clock组件需要设置一个计时器，并且需要每秒更新UI。理想情况下我们希望只编写一次代码，便可以让Clock组件自我更新：

```
ReactDOM.render(
  <Clock />
  document.getElementById('root')
);
```

我们需要在Clock组件中添加“state”来实现这个功能。
State与props类似，但是state是私有的，并且完全受控于当前组件。

二、将函数组件转换成class组件
我们通过五步想Clock的函数组件转换成class组件：
1.创建一个同性的ES6 class，并且继承于React.Component。
2.添加一个空的render()方法。
3.将函数体移动到render()方法之中。
4.在render()方法中使用this.props替换props。
5.删除剩余的空函数声明。
```
class Clock extends React.Component{
  render(){
    return(
	  <div>
        <h1>Hello, world!</h1>
	    <h2>It is {this.props.date.toLocaleTimeString()}. </h2>
      </div>
    );
  }
}
```

现在Clock组件被定义为class而不是函数。我们就可以使用如state或生命周期方法等很多其他特性。

三、向Class组件中添加局部的state
通过以下三步将date从props移动到state中：
1，把render()方法中的this.props.date替换成this.state.date：
```
class Clock extends React.Component{
  render(){
    return(
	<div>
	  <h1>Hello, world!</h1>
	  <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
	</div>
	);
  }
}
```

2.添加一个class构造函数，然后在该函数中为this.state赋初值：
```
class Clock extends React.Component{
  constructor(props){
    super(props);
	this.state = {date: new Date()};
  }

  render(){
    return(
	<div>
	  <h1>Hello, world!</h1>
	  <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
	</div>
	);
  }
}
```

通过以下方式将props传递到父类的构造函数中：
```
  constructor(props){
    super(props);
	thsi.state = {date: new Date()};
  }
```

class组件应该始终使用props参数来调用父类的构造函数。
3.移除<Clock />元素中的date属性：
```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

四、将生命周期方法添加到class中
在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

- 当Clock组件第一次被渲染到DOM中的时候，就为其设置一个计时器。这在React中被称为“挂载（mount）”。
- 当DOM中CLock组件被删除的时候，应该清除计时器。这在React中被称为“卸载（unmount）”。

```
class Clock extends React.Component{
  construtor(props){
    super(props);
	this.state = {date: new Date()};
  }
}

  componentDidMount(){

  }

  componentWillUnmount{

  }

  render(){
     return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

componentDidMount()方法会在组件已经被渲染到DOM中后运行，所以在这里设置计时器：
```
componentDidMount(){
  this.timerID = setInterval(
  () => this.tick(),
  1000
  );
}
```

接下来吧计时器的ID保存在this之中。
在componentWillUnmount()生命周期中清除计时器：
```
componentWillUnmount(){
  clearInterval(this.timerID);
}
```

最后实现一个叫tick()的方法，Clock组件每秒都会调用它。
使用this.setState()来时刻更新组件state:
```
tick(){
  this.setState({
    date: new Date()
  });
}
```

我们来快速概括一下发生了什么和这些方法的调用顺序：
1.当<Clock />被传给ReactDOM.render()的时候，React会调用Clock组件的构造函数。因为Clock需要显示当前的时间，所以他会用一个包含当前时间的对象来初始化this.state。
2.之后React会调用组件的render()方法。这就是React确定该在页面上展示什么的方式。然后React更新DOM来匹配Clock渲染的输出。
3.当Clock的输出被插入到DOM中后，React就会调用mponentDidMount()生命周期方法。在这个方法中，Clock组件向浏览器请求设置一个计时器来每秒调用一次组件的tick()方法。
4.浏览器每秒都会调用一次tick()方法。在这个方法中，Clock组件会通过调用setState()方法来计划进行一次UI更新。得益于setState()的调用，React能够知道state已经改变了，然后会重新调用render()方法来确定页面上该显示什么。这一次，render()方法中的this.state.date就不一样了，如此以来就会渲染输出更新过的时间。React也会相应的更新DOM。
5.一旦Clock组件从DOM中被移除，React就会调用componentWillUnmout()生命周期方法，这样计时器就停止了。

### 正确的使用State

关于setState()应该了解三件事：
1.不要直接修改State
```
//错误的
this.state.coment = 'Hello';

//正确的，应该使用setState():
this.setState({coment: 'Hello'});
```

构造函数是唯一可以给this.state赋值的地方。
2.State的更新可能是异步的
处于性能考虑，React可能会把多个setState()调用合并成一个调用。因为this.props和this.state可能会异步更新，所以不能依赖他们的值来更新下一个状态。

例如，此代码可能会无法更新计数器：

```
//错误的
this.setState({
  counter: this.state.counter + this.props.increment,
});

//正确的
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

这是让setState()接受一个函数而不是一个对象。这个函数用上一个state作为第一个参数，将此次更新被应用时的props作为第二个参数。

3.State的更新会被合并
当调用setState()的时候，React会把你提供的对象合并到当前的state。

例如，state包含几个独立的变量：
```
contructor(props){
  super(props);
  this.state = {
    posts: [],
	comments: [],
  }
}
```

然后可以分别调用setState()来单独的更新它们：
```
componentDidMount(){
  fetchPosts().then(response =>{
  this.setState({
    post: response.post
  });
});

  fetchComments().then(response =>{
    this.setState({
	  comments: response.comments
	});
  });
}
```

这里的合并是浅合并，所以this.setState({commnet})完整的保留了this.state.posts，但是完全替换了this.state.comment。

### 数据是向下流动的
不管是父组件还是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是class组件。

这就是为什么成state为局部的或是封装的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的state作为props向下传递到它的子组件中：
`<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
这对于自定义组件同样适用：
`<FormattedDate date = {this.state.date} />`
FormattedDate组件会在其props中接收参数date，但是组件本身无法知道它是来自于Clock的state，或是Clock的props，还是手动输入的：
```
function FormattedDate(props){
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

这通常会被叫做“自上而下”或是“单向”的数据流。任何的state总是所属于特定的组件，而且从该state派生的任何数据或UI只能影响树中“低于”它们的组件。

如果把一个以组件构成的树想象成一个props的数据瀑布的话，那么每一个组件的state就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。

为了证明每个组件都是真正独立的，我们可以创建一个渲染三个Clock的APP组件：
```
function APP(){
  return(
    <div>
	  <Clock />
	  <Clock />
	  <Clock />
	</div>
  );
}

ReactDOM.render(
  <APP />,
  docuement.getElementById('root')
);
```

每个Clock组件都会单独设置它自己的计时器并且更新它。

在React应用中，组件是有状态组件还是无状态组件属于组件实现的细节，它可能会随着时间的推移而改变。你可以在有状态的组件中使用无状态的组件，反之亦然。

## 事件处理
React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：

- React事件的命名采用小驼峰式，而不是纯小写。
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。
- 在React中不能通过返回false的方式阻止默认行为。必须显式的使用preventDefault。

### 向事件处理程序传递参数
在循环中，通常我们会为事件处理函数传递额外的参数。例如，若id是你要删除那一行的ID，以下两种方式都可以向事件处理函数传递参数：

```
<button onClick = {(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick = {this.deleteRow.bind(this, id)}>Delete</button>
```

上述两种方式是等价的，分别通过箭头函数和Function.prototype.bind实现。

在这两种情况下，React的时间对象会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过bind的方式，事件对象以及更多的参数将会被隐式的进行传递。

## 条件渲染
在React中，可以创建不同的组件来封装各种你需要的行为。然后依据应用不同的状态，你可以值渲染对象状态下的部分内容。

React中的条件渲染和JavaScript中的一样，使用JavaScript运算符`if`或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

以下两个组件：
```
function UserGreeting(props){
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props){
  return <h1>Please sign up.</h1>;
}
```

在创建一个Greeting组件，它会根据用户是否登录来决定显示上面的哪一个组件。
```
function Greeting(props){
  const isLoggedIn = props.isLoggedIn;
  if(isLoggedIn){
    return <UserGreeting />;
  }
  return <GusetGreeting />
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}
  <Greeting isLoggedIn = {false} />
  document.getElementById('root')
);
```

这个示例根据isLoggedIn的值来渲染不同的问候语。

### 元素变量
你可以使用变量来储存元素。它可以帮助你有条件地渲染组件的一部分，而其他的渲染部分并不会因此而改变。

以下两个组件分别代表了注销和登录按钮：
```
function LoginButton(props){
  return(
  <button onClick = {props.onClick}>
    Login
  </button>
  );
}

function LogoutButton(props){
  return(
  <button onClick = {props.onClick}>
    Logout
  </button>
  );
}
```

在下面的示例中，我们将创建一个名叫`LoginControl`的有状态的组件。

它将根据当前的状态来渲染`<LoginButton />`或者`<LogoutButton />`。同时它还会渲染上一个示例中的`<greeting />`。
```
class LoginControl extends React.Component{
  contructor(props){
  super(props);
	this.handleLoginClick = this.handleLoginClick.bind(this);
	this.handleLogoutClick = this.handleLogoutClick.bind(this);
	this.state = {isLoggedIn: false};
  }

  handleLoginClick(){
    this.setState({isLoggedId: true});
  }

  handleLogoutClick(){
    this.setState({isLoggedIn: false});
  }

  render(){
    const inLoggedIn = this.state.isLoggedIn;
	let button;

	if(isLoggedIn){
	  button = <LoginButton onClick = {this.handleLogoutClick} />;
	} else{
	  button = <LoginButton onClick = {this.handleLoginClick} />;
	}

	return(
	<div>
	  <Greeting isLoggedIn = {inLoggedIn} />
	  {button}
	</div>
	);
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

声明一个变量并使用if语句进行条件渲染是不错的方式，但有时可能会想使用更为简洁的语法。下面有几种在JSX中内联条件渲染的方法。

### 与运算符 &&
通过花括号包裹代码，你可以在JSX中嵌入任何表达式。这也包括JavaScript中的逻辑与（&&）运算符。它可以很方便地进行元素的条件渲染。
```
function Mailbox(props){
  const unreadMessages = props.unreadMessages;
  return(
  <div>
    <h1>Hello!</h1>
	{unreadMessages.length > 0 &&
	  <h2>
	    You have {unreadMessage.length} unread messages.
	  </h2>
	}
  </div>
  );
}

const messages = ['React', 'Re:React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages = {messages} />,
  document.getElementById('root')
);
```

之所以可以这样做，是因为在JavaScript中，`true && expression`总是返回`expression`，而`false && expression`总是返回`false`。
因此，如果条件是`true`，`&&`右侧的元素就会被渲染，如果是`false`，React会忽略并跳过它。

### 三目运算符

另一种内联条件渲染的方法是使用JavaScript中的三目运算符`condition ? true : false`。

在这个示例中，我们用它来条件渲染一小段文本：
```
render(){
  const isLoggedIn = this.state.isLoggedIn;
  return(
    <div>
	  The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
	</div>
	);
}
```

需要注意的事，如果条件变得过于复杂，那应该考虑如何提取组件。

### 阻止组件渲染
在极少数情况下，我们可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让`render`方法直接返回`null`，而不进行任何渲染。

下面的示例中，`<Warningbanner />`会根据`warn`的值来进行条件渲染。如果`warn`的值是`false`，那么组件则不会渲染：
```
function WarningBanner(props){
  if(!props.warn){
    return null;
  }

  return(
    <div className="warning">
	  Warning!
	</div>
	);
}

class Page extends React.Component{
  construtor(props){
  super(props);
	this.state = {showWarning: true};
	this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick(){
    this.setState(state => ({
	showWarning: !state.showWarning
	}));
}

  render(){
    return(
	  <div>
	  	<WarningBanner warn = {this.state.showWarning} />
		<button onClick = {this.handleToggleClick}>
		  {this.state.showWarning ? 'Hide' : 'Show'}
		</button>
	  </div>
	);
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

在组件的`render`方法中返回`null`并不会影响组件的生命周期。例如，上面这个示例中，`componentDidUpdate`依然会被调用。

## 列表 & Key

### 渲染多个组件
我们可以使用`{}`在JSX内构建一个元素集合。

我们使用JavaScript中的`map()`方法来遍历`numbers`数组。将数组中的每个元素变成`<li>`标签，最后我们将得到的数组赋值给`listItems`：
```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

我们把整个`listItems`插入到`<ul>`元素中，然后渲染进DOM：
```
reactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

### 基础列表组件
我们可以把前面的例子重构成一个组件，这个组件接收`numbers`数组作为参数并输出一个元素列表。
```
function NumberList(props){
  const numbers = props.numbers;
  const listItems = numbers.map((number) => 
    <li>{number}</li>
  );
  renturn(
  <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

当我们运行这段代码，将会看到一个警告`a key should be provided for list item`，意思是当你创建一个元素时，必须包括一个特殊的`key`属性。

让我们通过以下代码为每个列表元素分配一个`key`属性来解决上边的那个警告：
```
<li key={number.toString()}>
  {number}
</li>
```

### Key
Key帮助React识别哪些元素改变了，比如添加或删除，所以应该为数组中的每一个元素赋予一个确定的标识。

一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。通常我们使用来自数据id作为元素的key：

当元素没有确定id的时候，万不得已可以使用元素算因作为key。但这样做会导致性能变差，还可能引起组件状态的问题。

### 用可以提取组件
元素的key只有放在就近的数组上下文中才有意义。

比方说，如果提取出一个ListItem组件，应该把key保留在数组中的`<ListItem />`元素上，而不是放在`ListItem`组件中的`<li>`元素上。

一个好的经验法则是：在`map()`方法中的元素需要设置key属性。

### key只是在兄弟节点之间必须唯一
数组元素中使用的key在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组是，我们可以使用相同的key值：
```
function Blog(props){
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
	    <li>
	    	{post.title}
	    </li>
	  )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
	  <h3>{post.title}</h2>
	  <p>{post.content}</P>
	</div>
  );
  return(
    <div>
	  {sidebar}
	  <hr />
	  {content}
	</div>
  );
}

content posts = [
  {id: 1, title = 'Hello World', content = 'Welcome to learning React!'},
  {id: 2, title = 'Installation', content = 'You can install React from npm.'}
];

ReactDOM.render(
  <Blog posts={posts} />
  document.getElementById('root')
);
```

key会传递信息给React，但不会传递给你的组件。如果你的组件中需要使用`key`属性的值，请用其他属性名显式传递这个值：
```
const content post.map((post) =>
  <Post
    key={post.id}
		id={post.id}
		title={post.title} />
);
```

React组件可以读出`props.id`，但是不能读出`props.key`。

### 在JSX中嵌入map()
上边的例子中，我们声明了一个单独的`listItems`变量并将其包含在JSX中：
```
function NumberList(props){
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem
	  key={number.toString()}
	  value={number} />
  );
  return(
    <ul>
	  {listItems}
	</ul>
	);
}
```

JSX允许在大括号中嵌入任何表达式，所以我们可以内联`map()`返回的结果：
```
function NumberList(props){
  const numbers = props.numbers;
  return(
    <ul>
	  {numbers.map((number) =>
	    <ListItem key={number.toString()}
		          value={number} />
	  )}
	</ul>
	);
}
```

在需要的时候可以为了提高可读性提取出一个变量。

## 表单
在React里，HTML表单元素的工作方式和其他的DOM元素有些不同，这是因为表单元素通常会保持一些内部的state。例如这个纯HTML表单只接受一个名称：
```
<form>
  <label>
    名字：
	<input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

此表单具有默认的HTML表单行为，即在用户提交表单后浏览到新页面。如果在React中执行相同的代码，它依然有效。但大多数情况下，使用JavaScript函数可以很方便的处理表单的提交，同时还可以访问用户填写的表单数据。实现这种效果的标准方式就是使用“受控组件”。

### 受控组件
在HTML中，表单元素（如`<input>`、`<textarea>`、`select`)之类的表单元素通常自己维护state，并根据用户输入进行更新。而在React中，可变状态（mutable state）通常保存在组件的state属性中，并且只能通过使用`setState()`来更新。

我们可以把两者结合起来，使React的state成为“唯一数据源”。渲染表单的React组件还控制着用户输入过程中表单发生的操作。被React以这种方式控制取值的表单输入元素就叫“受控组件”。

例如，如果我们想让前一个示例提交时打印出名称，我们可以将表单写为受控组件：
```
class NameForm extends React.Component{
  constructor(props){
  super(props);
  this.state = {value: ''};
	
  this.handleChange = this.handleChange.bind(this);
  this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event){
    this.setState({value: event.target.value});
  }

  handleSubmit(event){
    alert('提交的名字：' + this.state.value);
    event.preventDefault();
  }

  render(){
    return(
      <form onSubmit={this.handleSubmit}>
        <label>
          名字：
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

由于在表单元素上设置了`value`属性，因此显示的值始终为`this.state.value`，这使得React的state成为唯一数据源。由于`handlechange`在每次按键时都会执行并更新React的state，因此显示的值将随着用户输入而更新。

对于受控组件来说，每个state突变都有一个相关的处理函数。这使得修改或验证用户输入变得简单。例如，如果我们要强制要求所有名称都用大写字母书写，我们可以将`handlechange`改写为：
```
handleChange(event){
  this.setState({value: event.target.value.toUpperCase()});
}
```

### textarea标签
在HTML中，`<textarea>`元素通过其子元素定义其文本：
```
<textarea>
  你好，这是在 text area里的文本
</textarea>
```

而在React中，`<textarea>`使用`value`属性代替。这样，可以使得使用`<textarea>`的表单和使用单行input的表单非常类似。

### select标签
在HTML中，`<select>`创建下拉列表标签。例如，如下HTML创建了水果相关的下拉列表：
```
<select>
  <option value="grapefruit">葡萄柚</option>
  <option value="lemon">柠檬</option>
  <option selected value="coconut">椰子</option>
  <option value="mango">芒果</option>
</select>
```

由于`selected`属性的缘故，椰子选相默认被选中。React并不会使用`selected`属性，而是在根`select`标签上使用`value`属性。这在受控组件中更便捷，因为只需要在根标签中更新它。例如：
```
class FlavorForm extends React.Component{
  constructor(props){
  super(props);
	this.state = {value: 'coconut'};

	this.handleChange = this.handleChange.bind(this);
	this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event){
    this.setState({value: event.target.value});
  }

  handleSubmit(event){
    alert('你最喜欢的风味是：' + this.state.value);
	event.preventDafault();

  }

  render(){
    return(
	  <form onSubmit={this.handleSubmit}>
	    <label>
		  选择你喜欢的风味：
		  <select value={this.state.value} coChange={this.handleChange}>
		    <option value="grapefruit">葡萄柚</option>
		    <option value="lemon">柠檬</option>
		    <option value="coconut">椰子</option>
		    <option value="mango">芒果</option>
		  </select>
		</label>
		<input type="submit" value="提交" />
	  </form>
	);
  }
}
```

总的来说，这使得`<input type="text">`，`textarea>`，`<select>`之类的标签都非常相似——它们都接受一个`value`属性，你可以使用它来实现受控组件。

### 文件input标签
在HTML中，`<input type="file">`允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用JavaScript的File API进行控制。
```
<input type="file" />
```

因为它的value只读，所以它是React中的一个非受控组件。

### 处理多个输入
当需要处理多个`input`元素时，我们可以给每个元素添加`name`属性，并让处理函数根据`event.target.name`的值选择要执行的操作。

例如：
```
class Reservation extends React.Component {
  constructor(props) {
  super(props);
  this.state = {
    isGoing: true,
    numberOfGuests: 2
  };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          参与：
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          来宾人数：
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    )
  }
}
```

这里使用了ES6计算属性名称的语法更新给定输入名称对应的state值：

```
this.setState({
  [name]: value
});
```

等同于ES5：
```
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

另外，由于`setState()`自动将部分state合并到当前state，只需调用它更改部分state即可。

### 受控输入空值
在受控组件上指定的value的prop可以防止用户更改输入。如果指定了`value`，但输入仍可编辑，则可能是意外地将`value`设置为`undefined`或`null`。

### 受控组件的替代品
有时使用受控组件会很麻烦，因为你需要为数据变化的每种方式都编写时间处理函数，并通过一个React组件传递所有的输入state。当你将之前的代码库转换为React或将React应用程序与飞React库集成时，这可能会令人厌烦。在这些情况下，你可能希望使用非受控组件，这是实现输入表单的另一种方式。

## 状态提升
通常，多个组件需要反映相同的数据变化，这是我们建议将共享状态提升到最近的共同父组件中去。

这次创建一个用于计算水在给定温度下是否会沸腾的温度计算器。

首先创建一个名为`BoilingVerDict`的组件开始，它接受`celsius`温度作为一个prop，并据此打印出该温度是否足以将水沸腾的结果。
```
function BoilingVerdict(props){
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water woulf not boil.</p>;
}
```

接下来创建一个名为`Calculator`的组件。它渲染了一个用书输入温度的`<input>`，并将其值保存在`this.state.temperature`中。

另外，它根据当前输入值渲染`BoilingVerdict`组件。
```

