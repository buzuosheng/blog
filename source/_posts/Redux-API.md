---
title: Redux API
date: 2019-12-18 20:25:49
tags:
---

## <a name="reduxapi">[Redux API](#reduxapi)</a>

​	Redux的API非常少。Redux定义了一系列的约定（contract），同时提供少量辅助函数来把这些约定整合到一起。

​	Redux只关心如何管理state。在实际的项目中，还需要使用UI绑定库如[react-redux](#reactapi)。

顶级暴露的方法：

- [createStore(reducer, [preloadedState], [enhancer])](#create)
- [combineReducers(reducers)](#combine)
- [applyMiddleware(...middlewares)](#apply)
- [bindActionCreators(actionCreators, dispatch)](#bind)
- [compose(...functions)](#compose)

Store API

- [Store](#store)
  - [getState()](#getState)
  - [dispatch(action)](#patch)
  - [subscribe(listener)](#subscribe)
  - [getReducer()](#getReducer)
  - [replaceReducer(nextReducer)](#replace)

ES6引入：

``` js
import { createStore } from 'redux';
```

### <a name="create">[createStore](#create)</a>

``` js
createStore(reducer, [preloadedState], enchancer)
```

​	创建一个Redux store来存放应用中所有的state。应用中有且只有一个store。

**参数**：

1、`reducer`*(Function)*：接收两个参数，分别是当前的state树和要处理的action，返回新的state树。

2、`[preloadedState]`*(any)*：初始state。

3、`enhancer`*(Function)*：Store enhancer是一个组合store creator的高阶函数，返回一个新的强化过的store creator。

**返回值**：

`store`：保存了应用所有state的对象。改变state的唯一方法是dispatch action。也可以subscribe 监听state的变化来更新UI。

### <a name="store">[Store](#store)</a>

​	Store就是用来维持应用所有的state树的一个对象。改变store内state的唯一途径是对它dispatch一个action。

​	Store不是类，只是有几个方法的对象。创建store只需要把根部的reducing函数传递给`createStore`。

**Store方法**：

- [`getState()`](#getState)
- [`dispatch(action)`](#dispatch)
- [`subscribe(listener)`](#subscribe)
- [`replaceReducer(nextReducer)`](#replace)

-----

<a name="getState">[**`getState()`**](#getState)：</a>

​	返回当前的state树。它与store的最后一个reducer返回值相同。

​	**返回值 ** *(any)*：应用当前的state树。

----

<a name="dispatch">[**`dispatch(action)`**](#dispatch)：</a>

​	分发action，这是触发state改变的唯一途径。会使用当前`getState()`的结果和传入的`action`以同步方式的调用store的reduce函数。返回值会被作为下一个state。从现在开始，这就成为了`getState()`的返回值，同时变化监听器（change listener）会被触发。

​	**参数**：

​		1、`action`(*Object*)：描述应用变化的普通对象。

​	**返回值**：

​	(Object)：要dispatch的action。

-----

<a name="subscribe">[**`subscribe(listener)`**](#subscribe)：</a>

​	添加一个变化监听器。每当dispatch action的时候就会执行。state树的一部分可能已经改变，可以使用`getState()`获取当前的state。

​	**参数**：

​		1、`listener`(*Function*)：每当dispatch action的时候都会执行的回调。

​	**返回值**：

​	(Function)：一个可以解绑变化监听器的函数。

-----

<a name="replace">[**`replaceReducer(nextReducer)`**](#replace)：</a>

​	替换当前用来计算state的reducer。

​	**参数**：

​		1、`reducer`(*Function*)：store会使用的下一个reducer。

### <a name="combine">[combineReducers ](#combine) </a>

``` js
combineReducers(reducers)
```

​	把一个由多个不同reducer函数作为value的object合并成一个最终的reducer函数，然后就可以对这个reducer调用`createStore`方法。

​	合并后的reducer可以调用各个子reducer，并把它们返回的结果合并成为一个state对象。**由`combineReducers()`返回的state对象，将会传入每个reducer返回的state按其传递给`combineReducers()`时对应的key进行命名**。

​	**参数**：

​		1、`reducers`(*Object*)：一个对象，它的value对应不同的reducer函数，这些函数后面会被合并成一个。

​	**返回值**：

​	(*Function*)：一个调用`reducers`对象里所有reducer的reducer，并且构造一个与`reducers`对象结构相同的state对象。

### <a name="apply">[applyMiddleware](#apply)</a>

``` js
applyMiddleware(...middleware)
```

​	使用包含自定义功能的middleware来扩展Redux是一种推荐的方式。Middleware可以让你包装store的`dispatch`方法来达到你想要的目的。

​	**参数**：

​		1、`...middleware`(*arguement*)：遵循Redux middleware API的函数。

​	**返回值**：

​	(*Function*)：一个应用了middleware后的store enhancer。这个store enhancer的签名是`createStore => createStore`，最简单的使用方法就是直接作为最后一个`enhancer`参数传给`createStore()`函数。

### <a name="bind">[bindActionCreators](#bind) </a>

``` js
bindActionCreators(actionCreators, dispatch)
```

​	把一个value为不同action creator的对象，转成拥有同名key的对象。同时使用`dispatch`对每个action creator进行包装，以便可以直接调用它们。

**参数**：

​	1、`actionCreator`(*Function or Object*)：一个action creator，或者一个value是action creator的对象。

​	2、`dispatch`(*Function*)：一个由Store提供的`dispatch`函数。

**返回值**：

​	(*Function or Object*)：一个与原对象类似的对象，只不过这个对象的value都是会直接dispatch原action creator返回的结果的函数。如果传入一个单独的函数作为`actionCreators`，那么返回的结果也是一个单独的函数。

### <a name="compose">[compose](#compose) </a>

``` js
compose(...functions)
```

​	从左到右来组合多个函数。

**参数**：

​	1、(*arguement*)：需要合成的多个函数。

**返回值**：

​	(*Function*)：从左到右把接收到的函数组合成的最终的函数。

## <a name="reactapi">[React-Redux API](#reactapi)</a>

### 安装

​	Redux和React之间没有关系，Redux支持React，需要安装react-redux。

``` powershell
npm install --save react-redux
```

## <a name="api">API</a>

``` js
<Provider store>
```

​	`<Provider store>`使组件层级中的`connect()`方法都能获得Redux store。正常情况下，根组件应该嵌套在`<Provider store>`中才能使用`connect()`方法。

**属性**：

- `store`(*Redux Store*)：应用程序中唯一的Redux store对象。
- `children`(*ReactElement*)：组件层级的根组件。

-----

``` js
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])

```

​	连接React组件和Redux Store。连接操作不会改变原来的组件类。反而**返回**一个新的已与Redux Store连接的组件类。

**参数**：

- `[ mapStateToProps(state, [ownProps]): stateProps ]`(*Function*)：如果定义该参数，组件将会监听Redux Store的变化。
- `[ mapDispatchRoProps(dispatch, [ownProps]: dispatchProps) ]`(*Object or Function*)：如果传递的是一个对象，那么每个定义在该对象的函数都会被当做Redux action creator，对象所定义的方法名将作为属性名；每个方法将返回一个新的函数，函数中的`dispatch`方法会将action creator的返回值作为参数执行。这些属性会被合并到组件的props中。
- `[ mergeProps(stateProps, dispatchProps, ownProps): props ]`(*Function*)：如果指定了这个参数，`mapStateToProps()`与`mapDispatchToProps()`的执行结果和组件自身的`props`将传入到这个回调函数中。该回调函数返回的对象将作为props传递到被包装的组件中。
- `[ options ]`(*Object*)：如果指定这个参数，可以定制connector的行为。
  - `[ pure = ture ]`(*boolean*)：如果为true，connector将执行`shouldComponentUpdate`并且浅对比`mergeProps`的结果，避免不必要的更新。
  - `[ withRef = false ]`(*boolean*)：如果为true，connector会保存一个队被包装组件实例的引用，该引用通过`getWrappedInstance()`方法获得。

**返回值**：

​	根据配置信息，返回一个注入了state和action creator的React组件。

**静态属性**：

​	`WrappedComponent`(*component*)：传递到`connect()`函数的原始组件类。

**静态方法**：

​	`getWrappedComponent(): ReactComponent`

仅当`connect()`函数的第四个参数`options`设置了`{ withRef: true }`才返回被包装的组件实例。

