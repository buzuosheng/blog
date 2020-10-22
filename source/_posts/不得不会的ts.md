---
title: 不得不会的ts
date: 2020-10-12 22:44:45
tags: Typescript
description:
categories: 学习记录
---

## 简单了解

TypeScript是JavaScript的一个超集，用于补充和完善JavaScript所缺少的一部分功能。

TypeScript相当于是对JavaScript很多问题的解决方案，随便编码过程中语法不同，但在运行时还是会先编译成JavaScript。

TS对于JS的补充，或者说TS相对于JS，有哪些不同，做一张表简单总结一下。

|                 TS                 |             JS             |
| :--------------------------------: | :------------------------: |
|   强类型语言，支持静态和动态类型   | 弱类型语言，不支持静态类型 |
|         在编码时期就会报错         |     在运行时期才会报错     |
| 浏览器无法识别，在运行时会编译成JS |   可以在浏览器中直接执行   |
|        支持模块、泛型和接口        |   不支持模块、泛型和接口   |

强类型语言使得在编程过程中有更好的IDE支持，在开发阶段可以处理大量的bug。

接口和泛型使得代码易读易维护，从而是JS变成一种更“高级”的语言。

## TS的基础类型

TypeScript的基础类型如下：

- Boolean
- Number
- String
- Symbol
- Array
- Enum
- Any
- Unknown
- Tuple
- Void
- Null和Undefined
- Object、object、{}
- Never

TS与JS主要的区别为Enum、Any、Unknown、Tuple、Void、`Object, Object, {}`、Never。

**Enum**类型是**将变量可能取的值一一列举**，如人的性别。

**Any**类型是**类型不明确的变量使用的一种类型**，是所有类型的父类型。

**Unknown**类型是为了解决Any引入的类型，**Unknown类型的值只能赋值给Any和Unknown类型**。

**Tuple**类型是**另一种数组**，数组一般只能储存同种类型的值，**Tuple可以解决储存不同类型值问题**。

**Void**类型是**没有任何类型**，一般作为函数的返回值。

**object**类型是表示非原始类型，在JavaScript中的原始类型有`string, boolean, number, bigInt, symbol, null, undefined`；**Object**类型是所有Object类的实例的类型；**{}**类型描述了一个没有成员的对象。

**Never**类型表示的是那些永不存在的值。

## TS中的类型断言

**类型断言用于指定一个值的类型**。

类型断言的语法为：

``` ts
// <类型>值
let oneValue: any = "this is a string"
let getLength: number = (<string>oneValue).length

// 值 as 类型， 在React的tsx中必须使用该格式
let oneValue: any = "this is a string"
let getLength: number (oneValue as string).length
```

一般用法为**将联合类型的变量指定为其中某一具体变量**，**在不确定其类型就访问其方法或属性**等。

## TS的类型守卫

**类型保护是可执行运行检查的一种表达式，用于确保该类型在一定的范围内。**

目前有四种方式来实现类型保护：

- **in关键字**

``` ts
interface Admin {
  name: string;
  privileges: string[];
}

interface Employee {
  name: string;
  startDate: Date;
}

type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
  console.log("Name: " + emp.name);
  if ("privileges" in emp) {
    console.log("Privileges: " + emp.privileges);
  }
  if ("startDate" in emp) {
    console.log("Start Date: " + emp.startDate);
  }
}
```

- **typeof关键字**

``` ts
function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
      return Array(padding + 1).join(" ") + value;
  }
  if (typeof padding === "string") {
      return padding + value;
  }
  throw new Error(`Expected string or number, got '${padding}'.`);
}
```

- **instanceof关键字**

``` ts
interface Padder {
  getPaddingString(): string;
}

class SpaceRepeatingPadder implements Padder {
  constructor(private numSpaces: number) {}
  getPaddingString() {
    return Array(this.numSpaces + 1).join(" ");
  }
}

class StringPadder implements Padder {
  constructor(private value: string) {}
  getPaddingString() {
    return this.value;
  }
}

let padder: Padder = new SpaceRepeatingPadder(6);

if (padder instanceof SpaceRepeatingPadder) {
  // padder的类型收窄为 'SpaceRepeatingPadder'
}
```

- **自定义类型保护的类型谓词**

``` ts
function isNumber(x: any): x is number {
  return typeof x === "number";
}

function isString(x: any): x is string {
  return typeof x === "string";
}
```

## TS的联合类型

**联合类型是可以通过管道`|`将变量设置为多种类型，赋值时可以通过设置的类型来赋值。**

示例：

``` ts
let value: string | number
value = "value"	//value
value = 1		//1
```

联合类型数组：

``` ts
let arr: number[] | string[];
arr = [1,2,3]			//[1,2,3]
arr = ["1", "2", "3"]	//["1", "2", "3"]
```

## TS中的交叉类型

**交叉类型是将多个类型合并为一个类型。**通过使用`&`运算符可以实现这个操作。

类型的合并分为两种，

- 基础类型的合并

  基础类型的合并非常简单，对于相同类型的同名属性，直接合并，如果同名属性的类型不同，则会变为`never`类型。如下代码，当`P1`和`P2`都拥有相同的属性`b`，
  
  ``` ts
  interface P1 {
    a: boolean;
    b: string;
  }
    
  interface P2 {
    a: boolean;
    b: number;
  }
    
  type P = P1 & P2;
    
  type Pa = T['a']; // boolean
  type Pb = T['b']; // string & number, never
  ```


- 非基础类型属性

  **如果要合并的是非基础数据类型，是可以直接合并的**。

  ``` ts
  interface P1 { p1: boolean;}
  interface P2 { p2: string;}
  interface P3 { p3: number;}
  
  interface A { p: P1;}
  interface B { p: P2;}
  interface C { p: P3;}
  
  type P = A & B & C;
  let x: P = {
    p: {
      p1: true,
      p2: 'true',
      p3: 1
    }
  }
  ```

## TS中的函数
  TS作为JS的超集，由于变量都需要声明类型，函数部分也有很明显的不同。具体如下：

- 添加变量类型

  **在TS的函数中，参数和返回值的类型都需要声明**。如果有一个参数或返回值不同，就会变为另外一个函数。

  ``` ts
  function sum(a: number, b: number): number {
    return a + b
  }
  ```

- 参数

  参数分为四种，分别为**可选参数，必填参数，默认参数，剩余参数**。直接上代码看使用说明：

  ``` ts
  // 可选参数。 可选参数需要放在普通参数的后边，不然会报错。
  function sum(a: number, b: number, c?: number): number {
    return a + b
  }
  
  // 默认参数
  function sum(a: number=1, b: number): number {
    return a + b
  }
  
  // 剩余参数
  function push(array, ...items) {
    items.forEach(function (item) {
      array.push(item);
    });
  }
   
  let a = [];
  push(a, 1, 2, 3);
  ```

- 函数重载

  **函数重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。**

  ``` ts
  function add(a: number, b: number): number;
  function add(a: string, b: string): string;
  function add(a: string, b: number): string;
  function add(a: number, b: string): string;
  function add(a: Combinable, b: Combinable) {
    // type Combinable = string | number;
    if (typeof a === 'string' || typeof b === 'string') {
      return a.toString() + b.toString();
    }
    return a + b;
  }
  ```

## TS中的接口

**接口**也是一个非常重要的概念。TS中接口的主要作用是**对对象进行约束描述，对类的一部分行为进行抽象**。

声明一个接口使用`interface`关键字：

``` ts
interface Person {
  readonly name: string;	//readonly只读属性，限制只在创建时改变值。
  age ?: number;			//可选属性
  [propName: string]: any; 	//自定义属性
}
```

接口和类型别名的区别。

- 接口类型**只能描述对象和函数**，类型别名可以用于一些其他类型。

  ```ts
  // 对象
  interface Num {
    x: number;
    y: number
  }
  
  // 函数
  interface sum {
    (x: number, y: number): number
  }
  
  // type别名
  type Num = {
    x: number;
    y: number;
  }
  
  type sum = (x: number, y: number) => number
  
  type Num1 = { z: number;}
  type Number = Num | Num1
  
  type Date = { number, string}
  ```

- **接口和类型别名是不互斥的，但语法不同**。

  ``` ts
  // 接口继承接口
  interface P1 { x: number}
  interface P Pextends P1 { y: nubmer}
  
  // 类型别名继承类型别名
  type P1 = {x: number}
  type P = P1 & { y: number }
  
  // 接口继承类型别名
  type P1 = { x: number}
  interface P extends P1 { y: number}
  
  // 类型别名继承接口
  interface P1 { x: number}
  type P = P1 & { y: number}
  ```

- **接口可以多次定义，会自动合并为一个**，而类型别名不行。

  ``` ts
  interface P {x: number}
  interface P {y: number}
  
  const p: P {
    x: 1,
    y: 1
  }
  ```

 ## TS中的类

TS中使用`class`关键字定义一个类，声明该类创建的所有对象**共有的属性和方法**。

``` ts
class Person {
  // 构造函数，初始化操作
  constructor(name: string){
    this.name = name
  }
  // 静态属性
  static sex: string = "male";
    
  // 私有属性，使用#符号
  #name: string
  
  // 成员属性
  age: number;
    
  // 静态方法
  static getName() {
    return name;
  }
    
  // 成员方法
  getAge() {
    return this.age
  }
}
```

