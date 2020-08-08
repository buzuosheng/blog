---
title: React面试基础
date: 2020-04-13 11:59:42
tags:
---

## 1、React是什么

React是一个为数据提供渲染为HTML视图的开源JavaScript库。拥有虚拟DOM、组件化设计模式、声明式代码、单向数据流、使用JSX描述UI信息等特点。

## 2、 Real DOM和Virtual DOM

React不直接操作DOM，而是实现了Virtual DOM，组件DOM结构映射到这个虚拟DOM上。React在虚拟DOM上实现了diff算法，当要重新渲染组件的时候，会通过diff寻找到要变更的DOM节点，再把这个修改更新到浏览器实际上的DOM节点。

**虚拟DOM相当于在JS和真实DOM中间加了一个缓存，利用diff减少了没有必要的操作，从而提高了性能，本质上是JS对象。**

## 3、React中keys和diff算法

**keys是React用于追踪元素被修改、添加或移除的辅助标识。**我们需要保证元素的key在列表中具有唯一性，这样可以帮助React定位到正确的节点进行比较，从而大幅减少DOM操作的次数，提高性能。

**diff算法的作用是用来计算出Virtual DOM中被改变的部分，然后针对该部分进行原生DOM的操作，而不用重新渲染整个页面。**

diff算法有三种策略：

- tree diff：DOM节点跨层级的移动操作特别少，可以忽略不计。
- component diff：拥有相同类的两个组件生成相似的数据结构；拥有不同类的两个组件生成不同的树形结构。
- element diff：对于同一层级的一组子节点，通过唯一id区分。

diff算法原理：

- 将树形结构按照层级分解，只比较同级元素。
- 给列表结构的每个单元添加唯一的key属性，方便比较。
- React只会匹配相同class的component
- 合并操作，调用component的setState方法的时候，React将其标记为dirty，到每一轮事件循环结束，React检查所有标记dirty的component重新渲染。
- 选择性子树渲染。开发人员可以重新shouldComponentUpdate提高diff的性能。

## 4、React中的Element与Component

**ReactElement是描述屏幕上可见内容的数据结构，是对于UI对象的表述。**

**ReactComponent则是可以接受参数输入并且返回某个ReactElement的函数或者类。**

## 5、JSX是什么

**JSX是JavaScript XML的缩写，是一个JavaScript的语法扩展，本质上是JavaScript对象。**JSX可以很好的描述UI信息，但是浏览器无法直接读取，编译的过程中会将JSX转换成JavaScript的对象结构。

## 6、ES5、ES6、ES7、ES8对比

**ES5：**扩展了Object、Array、Function等功能

**ES6：**类、模块化、箭头函数、块级作用域、赋值解构、扩展运算符、Promise、新数据结构Set，Map，Symbol、字符串模板等

**ES7：**指数操作符、Array.prototype.includes()等

**ES8：**异步函数使用形式、异步编程机制Generator和async/await、Object.entries()、Object.values()、Object.getOwnPropertyDescriptors()等

## 7、props和state

**props是React中属性的简写，是不可变的，可以从父组件传入参数配置该组件。**

**state是用于组件保存、控制、修改自己的可变状态。**

没有设置state的组件叫无状态组件，设置了state的组件叫有状态组件。有状态组件通过继承component来构建，一个子类就是一个组件；无状态组件通过函数式声明来构建，一个函数就是一个组件。

## 8、通信

React中的组件通信有以下几种情况：

- 父子组件通信
- 兄弟组件通信
- 跨多层次组件通信
- 任意组件通信

**父子组件通信：父组件通过props传递参数给子组件，子组件通过调用父组件传来的函数传递数据给父组件。**

**兄弟组件通信：通过使用共同的父组件来管理状态和事件函数。一个组件通过父组件传来的函数修改父组件的状态，父组件再将状态传递给另一个子组件。**

**跨多层次组件通信：使用Context API。**

**任意组件：使用Redux或者Event Bus。**

## 9、生命周期函数

**getDefaultProps**：获取实例的默认属性。

**getInitialState**：获取每个实例的初始化状态。

**componentWillMount**：组件即将被装载、渲染到页面上。

**componentDidMount**：组件已经被装载到页面上。

**componentWillReceiveProps**：组件将要接收到属性的时候调用。

**shouldComponentUpdate**：组件接收到新属性或者新状态的时候。

**componentWillUpdate**：组件即将更新。

**componentDidUpdate**：组件已经更新。

**componentWillUnmount**：组件卸载。

## 10、React中的refs

**refs是React提供给我们的安全访问DOM元素或者某个组件实例的句柄。**我们可以为添加ref属性然后在回调函数中接受该元素在DOM树中的句柄，该值会作为回调函数的第一个参数返回：

``` react
class CustomForm exrends Component {
    handleSubmit = () => {
        console.log("Input Value:", this.input.value)
    }
    render() {
        return(
        <form onSubmit={this.handleSubmit}>
            <input type='text' ref={input => this.input = input} />
            <button type='submit'>Submit</button>
        </form>
        )
    }
}
```

代码中的input包含了一个ref属性，该属性声明的回调函数会接收input对应的DOM元素，我们将其绑定到this指针以便在其他类函数中使用。另外ref在函数式组件同样能够利用闭包暂存其值。

## 11、受控组件

`<input>`，`<textarea>`和`<select>`这样的表单会维护自己的状态，基于用户的输入来更新。**而在React中，可变状态通常保存在组件的state属性中，并且只能通过使用`setState()`来更新。**这样的组件就叫做受控组件。

## 12、高阶组件

**高阶组件就是一个以组件作为参数，并返回一个组件的函数。**高阶组件的作用就为了提高组件之间的代码复用。如果组件有某些相同的逻辑，那我们可以将这些逻辑抽离出来，放到高阶组件中进行复用，**高阶组件和参数组件使用props传递数据**。

## 13、Flux和Redux

**Flux是一种强制单向数据流的架构模式。**

Flux主要有这几个部分：

- Dispatcher调度：处理动作分发，维护store之间的依赖关系；
- Stores存储：数据和逻辑部分；
- Views：React组件，作为视图同时响应用户交互；
- Actions提供给dispatcher传递数据给store。

Flux工作流程：

1、用户访问View；

2、View发送出用户的Action；

3、Dispatcher收到Action，要求Store进行响应的更新；

4、Store更新后，发出change事件；

5、View收到change事件后，更新页面。

**Redux是JavaScript状态容器，提供可预测化的状态管理。**

Redux有三大原则：单一数据来源、State是只读的、使用纯函数进行更改。

**Redux原理是集中式管理，主要有三个核心方法：action、store、reducer。**

Redux工作流程：

1、应用调用store的dispatch接受action传入store；

2、reducer进行state操作；

3、应用通过store提供的getState获取最新的数据。

Flux和Redux主要区别在于**Flux有多个可以改变应用状态的store**，在Flux中dispatcher被用来传递数据到注册的回调事件；**在Redux中只能定义一个可更新状态的store**，redux把store和Dispatcher合并，结构更加简单清晰。

Redux的缺点：

- 一个组件所需要的数据，必须由父组件传过来，而不能向Flux一样直接从store获取。
- 当一个组件数据更新时，即使父组件不需要用到这个组件，夫组件还是会重新render。

## 14、React-Router

**React-Router是一个基于React之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与URL间的同步。**

Router用于定义多个路由，当用户定义特定的URL时，如果此URL与Router内定义的任何“路由”的路径匹配，则用户将重定向到该特定路由。

