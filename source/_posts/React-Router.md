---
title: React-Router
date: 2019-12-15 23:23:08
tags:
---

## 介绍

​	`react-router`被分为以下几部分：

- react-router是浏览器和原生应用中的通用部分。
- react-router-dom是用于浏览器的。
- react-router-native是用于原生应用的。

​	`react-router`是核心部分。`react-router-dom`提供了浏览器使用需要的定制组件。`react-router-native`则专门提供了在原生移动应用中需要用到的部分。

## 安装

​	开发web引用只需要安装`react-router-dom`。

``` powershell
npm install react-router-dom --save
```

## 三个props

### <a name="history">[history](#history)</a>

​	History是React Router的两大重要依赖之一，在不同的JavaScript环境中，`history`以多种形式实现了对于session历史的管理。

​	`history`对象通常会具有以下属性和方法：

- `length` - number类型，表示history堆栈的数量。
- `action` - string类型，表示当前的动作。比如`pop`、`replace`或`push`。
- `location` - object类型，表示当前的位置。
- `push(path, [state])` - function类型，在history堆栈顶加入一个新的条目。
- `replace(path, [state])` - function类型，替换在history堆栈中的当前条目。
- `go(n)` - function类型，将history堆栈中的指针向前移动`n`。
- `goBack()` - function类型，等同于`go(-1)`。
- `goForward()` - function类型，等同于`go(1)`。
- `block(promt)` - function类型，阻止跳转。

### <a name="match">[match](#match)</a>

​	match对象包含了`<Route path>`如何与URL匹配的信息。`match`对象包含以下属性：

- `params` - object类型，表示路径参数，通过解析URL中动态的部分获得的键值对。
- `isExact` - 为`true`时，整个URL都需要匹配。
- `path` - string类型，用来做匹配的路径格式。
- `url` - string类型，URL匹配的部分。

​	获取`match`对象：

- 在Route component中，以`this.props.match`方式。
- 在Route render中，以`({ match }) => ()`方式。
- 在Route children中，以`({ match }) => ()`方式。

### <a name="location">[location](#location)</a>

location是指当前的位置，下一步的位置，或者之前所在的位置。location的属性：

- `pathname` - string类型，URL路径。
- `search` - string类型，URl中查询字符串。
- `hash` - string类型，URL的hash分段。
- `state` - string类型，表示location中的状态，只会出现在browser history和memory history的环境里。

​	获取location对象的方式：

- 在Route component中，以`this.props.location`的方式获取。
- 在Route render中，以`({ location }) => ()`的方式获取。
- 在Route children中，以`({ location }) => ()`的方式获取。
- 在withRouter中，以`this.props.location`的方式获取。

## 路由组件

### Router

​	针对不同功能和平台，有集中不同的子类组件：

- `<BrowserRouter>`  浏览器的路由组件
- `<HashRouter>`  URL格式为Hash路由组件
- `<MemoryRouter>`  内存路由组件
- `<NativeRouter>`  Native的路由组件
- `<StaticRouter>`  地址改变的静态路由组件

### BrowserRouter组件

​	`BrowserRouter`主要用于浏览器中，也就是web应用中。当点击程序中的一个连接之后，`BrowserRouter`就会找出与这个`URL`匹配的`Route`，并将对应的组件渲染出来。`BrowserRouter`是用来管理组件的，应用程序的组件作为它的子组件而存在。

​	`BrowserRouter`组件提供的属性：

- `basename` - string类型，路由器 的默认根路径。
- `forceRefresh` - bool类型，在导航的过程中整个页面是否刷新。
- `getUserConfirmation` - function类型，当导航需要确认时执行的函数。默认是：`window.confirm`。
- `keylength` - number类型，`location.key`的长度。默认是6。
- `children` - node类型，渲染单一子组件。

### HashRouter

​	`HashRouter`使用的URL的hash来保持UI和URL的同步。使用hash的方式记录导航历史不支持`location.key`和`location.state`。跟`BrowserRouter`类似，也有`basename`、`getUserConfirmation`、`children`属性，而且是一样的。

### MemoryRouter

​	主要用在ReactNative这种非浏览器的环境中，因此直接将URL的history保存在了内容中。StaticRouter主要用于服务器端渲染。

### Route组件

​	<Route>组件是react router的最重要的组件，当location与Route的path匹配时渲染Route中的Component。如果多个Route匹配，那么这些Route的Component都会被渲染。

`Route`组件的属性：

- `path` - 字符串类型，它的值就是用来匹配url的。
- `component` - 它的值是一个组件，在`path`匹配成功之后会渲染这个组件。
- `exact` - 指明这个路由是不是排他匹配。
- `strict` - 指明路径只匹配以斜线结尾的路径。

可以代替`component`属性的属性：

- `render` - function类型，Route会渲染这个function的返回值，可以添加一些副作用。
- `children` - 即使没有匹配路径的时候，也总是会渲染。我们可以根据match参数来决定匹配的时候渲染什么，不匹配的时候渲染什么。

​	所有路由中指定的组件将被传入以下三个props：[location](#location)、[match](#match)、[history](#history)。

### Link组件

​	使用`<Link>`可以在React应用的不同页面之间跳转，只会加载页面里和当前url可以匹配的部分。`<Link>`组件需要用到`to`属性，用来指明目标组件的地址，地址可以是一个字符串，也可以是一个location对象。

​	replace属性设置为`true`时，点击链接后将使用新地址替换掉访问历史记录里面的原地址。设置为`false`时，点击链接后将在原有访问历史的基础上添加一个新纪录。

​	`NavLink`是一个特殊的`Link`，可以使用activeClassName来设置Link被选中时被附加的class，使用activeStyle来配置被选中时应用的样式。`exact`属性要求路径完全匹配才会附加class和style。

### Redirect组件

​	当这个组件被渲染时，location会被重写为Redirect的to制定到新location。它的一个用途是登陆重定向，比如用户在点击登录按钮并通过验证之后，将页面跳转到个人主页。

### Switch组件

​	渲染匹配地址（location）的第一个`<Route>`或者`<Redirect>`。相反的，每一个包含匹配地址的（location）的`<Route>`都会被渲染。

