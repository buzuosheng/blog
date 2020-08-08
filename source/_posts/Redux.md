---
title: Redux
date: 2019-12-17 20:41:08
tags:
---

## 思想

​	应用中所有的[state](#state)都以一个对象树的形式储存在一个单一的[store](#store)中。唯一能改变state的办法是触发[action](#action)，一个描述发生什么的对象。为了描述action如何改变state树，需要编写reducers。

​	Redux只有一个单一的store和一个根级的reduce函数(reducers)。随着应用的不断增大，应该把根级的reducer拆分成多个小的reducers，分别独立的操作state树的不同部分，而不是添加新的stores。

## 三大原则

### 单一数据源

​	**整个应用的state被储存在一棵object tree中，并且这个object tree只存在于唯一一个store中**。来自服务端的state可以在无需编写更多代码的情况下被序列化并注入到客户端中。

### State是只读的

​	**唯一改变state的方法就是出发action，action是一个用于描述已发生事件的普通对象**。这样确保了视图和网络请求都不能直接修改state，相反它们只能表达想要修改的意图。

### 使用纯函数来执行修改

​	**为了描述action如何改变state tree，需要编写reducers**。Reducer只是一些纯函数，它接受先前的state和action，并返回新的state。

## 基础

### <a name="action">[Action](#action)</a>

​	**Action**是把数据从应用传到store的有效载荷。它是state数据的唯一来源。一般通过`store.dispatch()`将action传到store。Action本质上是JavaScript普通对象。action内必须有一个字符串类型的`type`字段来表示将要执行的动作。多数情况下`type`会被定义成字符串常量。当应用规模变大时，可以使用单独的模块或文件存放action。

​	我们还需要添加一个`action index`字段来表示用户完成任务的动作序列号。因为数据是存放在数组中的，所以我们通过下标`index`哎引用特定的任务。而实际项目中一般会在新建数据的时候生成唯一的ID作为数据的引用标识。

``` js
{
    type: TOGGLE_TODO,
    index:5
}
```

​	**我们应该尽量减少在action中传递的数据**。传递`index`比传递整个任务对象要好。

### <a name="actionfunc">[Action创建函数](#actionfunc)</a>

​	**Action创建函数**就是生成action的方法。在Redux中的action创建函数只是简单的返回一个action：

``` js
function addTodo(text) {
    return {
        type: ADD_TODO,
        text
    }
}
```

​	这样做是action创建函数更容易被移植和测试。在传统的Flux中，当调用action创建函数时，一般会触发一个dispatch：

``` js
function addTodoWithDispatch(text) {
    const action = {
        type: ADD_TODO,
        text
    }
    dispatch(action)
}
```

​	Redux中只需要把action创建函数的结果传递给`dispatch()`方法即可触发一次dispatch过程：

``` js
dispatch(addTodo(text))
dispatch(completeTodo(index))
```

​	或是创建一个**被绑定的action**来自动dispatch：

``` js
const boundAddTodo = text => dispatch(addTodo(text))
const boundCompleteTodo = index => dispatch(completeTodo(index))
```

​	store里能直接通过`store.dispatch()`调用`dispatch()`方法，但是多数情况下会使用react-redux提供的`connect()`帮助器来调用。`BindActionCreators()`可以自动把多个action创建函数绑定到`dispatch()`方法上。

### <a name="reducer">[Reducer](#reducer)</a>

​	**Reducers**指定了应用状态的变化如何响应actions并发送到store，actions只是描述了有事情发生了这一事实，并没有描述应用如何更新state。

​	在Redux应用中，所有的state都被保存在一个单一对象中，在写代码前应该先想一下这个对象的结构。如何才能以最简的形式把应用的state用对象描述出来。

​	以todo应用为例，需要保存两种不同的数据：

- 当前选中的任务过滤条件；
- 完整的任务列表。

​	通常这个state树还需要存放其它的一些数据，以及一些UI相关的state。尽量把这些数据与UI相关的state分开。

``` js
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'keep all state in a single tree',
      completed: false
    }
  ]
}
```

**Action处理**

​	确定了state对象的结构，就可以开始开发reducer。reducer就是一个纯函数，接受旧的state和action，返回新的state。

``` js
(previousState, action) => newState
```

​	保持reducer纯净非常重要，所以**永远不要**在reducer中做这些事：

- 修改传入参数；
- 执行有副作用的操作；
- 调用非纯函数。

​	**只要传入参数相同，返回计算得到的下一个state就一定相同。没有特殊情况，没有副作用，没有API请求，没有变量修改，单纯执行计算。**

### <a name="store">[Store](#store)</a>

​	使用[action](#action)来描述“发生了什么”，使用[reducer](#reducer)来根据action更新state的用法。[Store](#store)就是把它们联系在一起的对象。Store有以下职责：

- 维持应用的state；
- 提供`getState()`方法获取state；
- 提供`dispatch()`方法更新state；
- 通过`subscribe(listener)`注册监听器；
- 通过`subscribe(listener)`返回的函数注销监听器。

​	**Redux应用只有一个单一的store**。当需要拆分数据逻辑时，应该使用reducer组合而不是创建多个store。

### <a name="shuju">[数据流](#shuju)</a>

​	**严格的单向数据流**是Redux结构的核心设计。

​	这意味着应用中所有的数据都遵循相同的声明周期，这样可以让应用变得更加可预测且同意理解。同时也鼓励做数据规范化，这样可以避免使用多个独立且无法相互引用的重复数据。

​	Redux应用中数据的声明周期遵循4个步骤：

1、调用`store.dispatch(action)`。

2、Redux store调用传入的reducer函数。

3、根reducer应该把多个子reducer输出合并成一个单一的state树。

4、Redux store保存了根reducer返回的完整state树。

### <a name="react">[搭配React](#react)</a>

​	Redux支持React、Angular、Ember、JQuery甚至是纯JavaScript。但是React允许以state函数的形式来描述界面，Redux通过action的形式来发起state变化。

​	安装React-Redux：

``` powershell
npm install --save react-redux
```

​	Redux的React绑定库是基于**容器组件和展示组件相分离**的开发思想，这个思想非常重要。

|               |          展示组件          |              容器组件              |
| :-----------: | :------------------------: | :--------------------------------: |
|     作用      | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用Redux |             否             |                 是                 |
|   数据来源    |           props            |          监听Redux state           |
|   数据修改    |    从props调用回调函数     |         向Redux派发actions         |
|   调用方式    |            手动            |       通常由React Redux生成        |

​	大部分的组件都应该是展示型的，但一般需要少数的几个容器组件把它们和Redux store连接起来。

​	例如，我们想要显示一个todo项的列表。一个todo项被点击后，会增加一条删除线并标记为completed。我们会显示用户增加一个todo字段。在footer里显示一个可切换的显示全部/只显示completed的/只显示incompleted的todos。

展示组件和他们的props：

- `TodoList`用于显示todos列表。
  - `todos: Array`以`{ text, completed }`形式显示的todo项数组。
  - `onTodoClick(index: number)`当todo项被点击时调用的回调函数。
- `Todo`一个todo项。
  - `text: string`显示的文本内容。
  - `completed: boolean`todo项是否显示删除线。
  - `onClick()`当todo项被点击时调用的回调函数。
- `Link`带有callback回调功能的链接。
  - `onClick()`当点击链接时会触发。
- `Footer`一个允许用户改变可见todo过滤器的组件。
- `App`根组件，渲染余下的所有内容。

这些组件只定义外观不关心数据来源和如何改变。传入什么就渲染什么。如果把代码从Redux迁移到别的结构。这些组件可以不做任何改动的直接使用。

容器组件：

​	还需要一些容器组件来把展示组件连接到Redux。例如，展示型的`TodoList`组件需要一个类似`VisibleTodoList`的容器来监听Redux store变化并处理如何过滤出要显示的数据。为了实现状态过滤，需要实现`FilterLink`的容器组件来渲染`Link`并在点击时触发对应的action：

- `VisibleTodoList`根据当前显示的状态来对todo列表进行过滤，并渲染`TodoList`。
- `FilterLink`得到当前过滤器并渲染`Link`。
  - `filter: string`就是当前过滤的状态。

其它组件：

​	有时候表单和函数严重耦合在一起，很难分清该使用容器组件还是展示组件：

- `AddTodo`含有“Add”按钮的输入框。

