---
title: Redux快速使用
date: 2020-08-18 21:33:49
tags: Redux
categories: 技术栈
---

## Redux

> Redux is a predictable state container for JavaScript apps.

[Redux](https://redux.js.org/introduction/getting-started)是一个js应用的可预测状态容器。

Redux是根绝Flux改进的一个状态管理器，主要用于处理跨层级组件通信的问题。

Redux有**三大原则**：

- 整个应用的state被存储在**单个**的对象树中（store）；
- 状态是**只读**的，只能通过actions改变状态；
- 使用**纯函数**进行更改状态(reducer)。

## Store

Store主要有四个方法：

- `getState()`：获取当前的state
- `dispatch(action)`：发出一个action
- `subscribe(listener)`：添加一个监听器
- `createStore(reducer, [preloadedState], [enhancer])`：创建store

在创建一个Store时可以添加中间件，如`redux-thunk`用于异步获取数据，`redux-devtools-extension`浏览器调试工具等。

[redux-thunk](https://github.com/reduxjs/redux-thunk)和[redux-devtools-extension](https://github.com/zalmoxisus/redux-devtools-extension)使用：

``` js
import { composeWithDevTools } from 'redux-devtools-extension'
import thunk from 'redux-thunk'

const middleware = [ thunk ]
if (process.env.NODE_ENV !== 'production') {
  middleware.push(createLogger())
}

const store = createStore(
  reducer,
  composeWithDevTools(applyMiddleware(...middleware))
)
```

在项目中添加`redux-devtools-extension`后再开发者调试工具中就会多出一个`redux`的选项。

![redux-devtools-extension](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMAyzibV3RAvyt4hr6DlwE3qDCGdDuATeySJWGg1uRRQdNhIcAhic2e2sRjPgrjqW29Cv606bmMahncw/0?wx_fmt=png)

## Reducer

reducer是一个纯函数，大致如下：

``` js
(preState, action) => newState
```

**禁止在reducer中修改传入参数；禁止执行有副作用的操作；禁止调用非纯函数。**

React的思想是将页面抽象为一个个组件，当两个组件是相互独立时，应该为每个组件创建单独的reducer，最后使用`combineReducers()`将多个reducer合并。

## react-redux

react-redux只有两个API。

react使用`react-redux`来绑定redux。将根组件包裹在`<Provider>`中，将store作为props传入，使项目中的任何组件都可以使用store。然后使用`connect()`函数真正的连接react的组件和redux的store。

``` js
export default connect(mapStateToProps)(App)
```

connect有四个参数：

- mapStateToProps
- mapDispatchToProps
- mergeProps
- options

1、`mapStateToProps(atate, ownProps)`允许我们将store中的数据作为props绑定到组件上。

第一个参数为Redux的store，第二个参数为App组件自身的参数。当store或ownProps发生变化时，mapStateToProps()函数会被调用，得出一个新的state，传递给App组件。

2、`mapDispatchToProps(dispatch, ownProps)`是将action作为props绑定到组件上。

Redux本身提供了`bindActionCreators`函数，将action包装成直接可被调用的函数，即调用该函数就会触发dispatch。

3、`mergeProps(stateProps, dispatchProps, ownProps)`

不管是stateProps还是dispatchProps，都需要和ownProps merge之后才会赋值给App，该函数实现了这个功能。但不传递该参数也可以，connect会使用`Object.assign`方法代替。

4、options

## 总结

redux的工作流程：

1、页面产生交互性行为，发出action；

``` js
store.dispatch(action)
```

2、Store调用Reducer；

``` js
var nextState = todoApp(preState, action)
```

3、State一旦有变化，Store就会调用监听函数。

``` js
store.subscribe(listener)
```

4、listener通过`store.getState()`获取状态，React可以触发重新渲染视图。

``` js
function listener() {
  let newState = store.getState();
  component.setState(newState)
}
```

![redux工作流程](https://mmbiz.qpic.cn/mmbiz_jpg/GY9ZJPx6bMAyzibV3RAvyt4hr6DlwE3qDtTwTSetvQ4icQ9fLzuYUeKPWmqv7DiaicrI30DtbrGRqQuFXF4Dl4mjbg/0?wx_fmt=jpeg)

