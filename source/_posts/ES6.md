---
title: ES6
date: 2019-07-15 00:39:05
tags:
---
# ES6

## let和const
ES2015（ES6）新增加了两个重要的JavaScript关键字：**let**和**const**。
let声明的变量只在let命令所在的代码块内有效。
const声明一个只读的常量，一旦声明，常量的值就不能改变。

### let命令
let命令和var命令的区别：
1、let是在代码块内有效，var是在全局范围内有效；
2、let只能声明一次（for循环计数器很适合用let），var可以声明多次；
3、let不存在变量提升，var会变量提升。

### const命令
const声明一个只读变量，声明之后不允许改变。意味着，一旦声明必须初始化，否则会报错。

其实const保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。

简单类型和复合类型保存值的方式是不同的。
对于简单类型（数值number、字符串string、布尔值boolean），值就保存在变量指向的那个内存地址，因此const声明的简单类型变量等同于常量。
而对于发杂类型（对象object，数组array，函数function），变量指向的内存的地址其实是保存了一个指向实际数据的指针。所以const只能保证指针是固定的，至于指针指向的数据结构的改变就无法控制了。

## ES6 Symbol
ES6引入了一种新的原始数据类型Symbol，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。
ES6数据类型除了Number、String、Object、null和undefined，还新增了Symbol。

### 基本用法
Symbol函数栈不能用new命令，因为Symbol是原始数据类型，不是对象。可以接受一个字符串作为参数，为新创建的Symbol提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。

### 使用场景
- 由于每一个Symbol的值都是不相等的，所以Symbol作为对象的属性名，可以保证属性不重名。

```
let sy = Symbol("key1");

//写法1
let syObject = {};
syObject = "kk";
console.log(syObject);  //{Symbol(key1): "KK"}

//写法2
let syObject = {
	[sy]: "kk"
};
console.log(syObject);	//{Symbol(key1): "kk"}

//写法3
let syObject = {};
Object.defineProperty(syObject, sy, {value: "kk"});
console.log(syObject);	//{Symbol(key1): "kk"}
```

Symbol作为对象属性名时不能用.运算符，要用方括号。因为.运算符后面是字符串，所以取到的事字符串sy属性而不是Symbol值sy属性。

**注意点**
Symbol值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问。但是不会出现在for…in、for…of的循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。如果要读取到一个对象的Symbol属性，可以通过Object.getOwnPropertySymbols()和Reflect.ownKeys()取到。

```
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);

Object.getOwnPropertySymbols(syObject);	//[Symbol(key1)]
Reflect.ownKeys(syObject);
```

- 定义常量

```
//ES5
const COLOR_RED = "red";
const COLOR_BLUE = "blue";

//ES6
const COLOR_RED = Symbol("red");
const COLOR_BLUE = Symbol("blue");

```
使用Symbol定义常量，这样就可以保证这一组常量的值都不相等。


### Symbol.for()和Symbol.keyFor()
有时候，我们希望重新使用同一个Symbol值，Symbol.for()方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数为名称的Symbol值。如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。

```
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2;	//true
```

Symbol()方法也会新建一个Symbol值，但没有登记机制，所以每次调用都会返回不同的值。

Symbol.keyFor()方法返回一个已登记的Symbol类型值的key。
```
let s1 = Symbol.for('foo');
Symbol.keyFor(s1)	//"foo"
```

## Map与Set
Map对象保存键值对。任何值（对象或者原始值）都可以作为一个键或一个值。

Maps和Objects的区别

- 一个Object的键只能是字符串或者Symbols，但一个Map的键可以是任意值。
- Map中的键是有序的（FIFO原则），而添加到对象中的键则不是。
- Map的键值对个数可以从size属性获取，而Object的键值对个数只能手动计算。
- Object都有自己的原型，原型链上的键名有可能和你自己在对象上设计的键名产生冲突。

### Map中的key
- key是字符串

```
var m = new Map();
var KeyString = "a string";

m.set(keyString, "String");

myMap.get(keyString);	//'String'
myMap.get("s string");	//'String'
//keyString === 'a string'
```

- key是对象

```
var m = new Map();
var keyObj = {};

myMap.set(keyObj, "object");

myMap.get(keyObj);	//'object'
myMap.get({});		//undefined
//keyObj !=={}
```

- key是函数

```
var myMap = new Map();
var keyFunc = function () {};

myMap.set(keyFunc, "和键keyFunc关联的值");

myMap.get(keyFunc);		//和键keyFunc关联的值
myMap.get(function() {});//undefined
//keyFunc !==function() {}
```

- key是NaN

```
var myMap = new Map();
myMap.set(NaN, "not a number");

myMap.get(NaN); //"not a number"

var otherNaN = Number("foo");
myMap.get(otherNaN);//"not a number"
```

### Map的迭代
对Map进行遍历，以下两个最高级。

- for…of

```
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");

for (var [key, value] of myMap) {
  console.log(key + " = " + value);
}
```

- forEach()

```
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");

myMap.forEach(function(value, key) {
  console.log(key + " = " + value);
}, myMap)
```

### Map对象的操作
- Map与Array的转换

```
var kvArray = [["key1", "value1"], ["key2", "value2"]];

var myMap = new Map(kvArray);

var outArray = Array.from(myMap);
```

- Map的克隆

```
var myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);
var myMap2 = new Map(muMap1);

console.log(original === clone);
```

- Map的合并
合并两个Map对象时，如果有重复的键值，则后面的会覆盖前面的。

```
var first = new Map([[1, 'one'], [2, 'two'], [3, 'three']]);
var second = new Map([[1, 'uno'], [2, 'dos']]);

var merged = new Map([...first, ...second]);
```

### Set对象
Set对象允许你存储任何类型的唯一值，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：

- +0与-0在存储判断唯一性的时候是恒等的，所以不重复；
- undefined与undefined是恒等的，所以不重复；
- NaN与NaN是不恒等的，但是在Set中只能存一个，不重复。

对象之间引用不同不同不恒等，即使值相同，Set也能存储。

### 类型转换
- Array

```
//Array转Set
var mySet = new Set(["value1", "value2", "value3"]);
/用...操作符，将Set转Array
var myArray = [...mySet];

//String转Set
var mySet = new Set('hello');//Set(4) {"h", "e", "l", "o"}
//注：Set中toString方法不能将Set转换成String
```

### Set对象作用
- 数组去重

```
var mySet = new Set([1, 2, 3, 4, 4]);
[...mySet];	//[1, 2, 3, 4]
```

- 并集

```
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 1]);
var union = new Set([...a, ...b]);	//{1, 2, 3, 4}
```

- 交集

```
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 1]);
var intersect = new Set ([...a].filter(x => b.has(x)));	//{2, 3}
```

- 差集

```
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 1]);
var diffetence = new Set([...a].filter(x => !b.has(x)));	//{2}
```

## Reflect和Proxy
Proxy与Reflect是ES6为了操作对象引入的API。
Proxy可以对目标对象的读取、函数调用等操作进行拦截，然后进行操作处理。它不直接操作对象，而是像代理模式，通过对象的代理对象进行操作，在进行这些操作时，可以添加一些需要的额外操作。
Reflect可以用于获取目标对象的行为，它与Object类似，但是更易读，为操作对象提供了一种更优雅的方式。

### 基本用法
一个Proxy对象由两个部分组成：target、handler。在通过Proxy构造函数生成实例对象时，需要提供这两个参数。
target即目标对象，handler是一个对象，声明了代理target的指定行为。

Reflect对象对某些方法的返回结果进行了修改，使其更合理。
Reflect对象使用函数的方式实现了Object的命令式操作。

## ES6字符串

### 子串的识别
ES6之前判断字符串是否包含子串，用indexOf方法，ES6新增了子串的识别方法。

- includes():返回布尔值，判断是否找到参数字符串。
- startsWith():返回布尔值，判断参数字符串是否在原字符串的头部。
- endsWith():返回布尔值，判断参数字符串是否在原字符串的尾部。

以上三个方法都可以接受两个参数，需要搜索的字符串，和可选的搜索起始位置索引。

### 字符串重复
repeat():返回新的字符串，表示将字符串重复指定次数返回。
```
console.log("hello,".repeat(2));	//"hello,hello,"
```

>如果参数是小数，向下取整；
>如果参数是0至-1之间的小数，等同于repeat零次；
>如果参数是NaN，等同于repeat零次；
>如果参数是负数或者Infinity，会报错；
>如果参数是字符串，则会先将字符串转化为数字。

### 字符串补全

- padStart：返回新的字符串，表示用参数字符从头部补全原字符串
- padEnd：返回新的字符串，表示用参数字符串从尾部补全字符串。

>1、以上方法接收两个参数，第一个参数指定生成的字符串的最小长度，第二个参数是用来补全的字符串。如果没有指定第二个参数，则默认用空格填充。
>2、如果指定的长度小于或等于原字符串的长度，则返回原字符串。
>3、如果原字符串加上补全字符串长度大于指定长度，则截去超出位数的补全字符串。

## ES6数值
数值的表示：
二进制表示新写法：前缀0b或0B。
八进制表示新写法：前缀0o或0O。

## ES6对象

### 属性的简洁表示法
ES6允许对象的属性直接写变量，这时候属性名是变量名，属性值是变量值。
```
const age = 12;
const name = "Amy";
const person = {age, name};
person	//{age: 12, name: "Amy"}
```

方法名也可以简写
```
const person = {
	sayHi(){
	  console.log("Hi");
	}
}
person.sayHi();
//等同于
const person = {
  sayHi:function(){
    console.log("Hi");
  }
}
person.sayHi();
```

### 属性名表达式
ES6允许用表示式作为属性名，但是一定要将表达式放在方括号里。
```
const obj = {
 ["he"+"llo"](){
   return "Hi";
 }
}
obj.hello();
```

### 对象的扩展运算符
扩展运算符(...)用于取出参数对象所有可比案例属性然后拷贝到当前对象。

可用于合并两个对象
```
let age = {age: 15};
let name = {name: "Amy"};
let person = {...age, ...name};
person;	//{age: 15, name: "Amy"}
```

### 对象的新方法
Object.assign(target,source_1,...)
用于将源对象的所有可枚举属性复制到目标对象中。

```
let target = {a: 1};
let object1 = {b: 2};
let object2 = {c: 3};
object.assgin(target, object1, object2);
target;	//{a: 1, b: 2, c: 3}
```

Object.is(value1, value2)
用于比较两个值是否严格相等，与（===）基本类似。

## ES6数组

### 数组创建
Array.of()
将参数中所有值作为元素形成数组。参数值可以为不同类型。
```
console.log(Array.of(1, 2, 3, 4));	//[1, 2, 3, 4]
console.log(Array.of(1, '2', ture));//[1, '2', ture]
```

Array.from()
将类数组对象或可迭代对象转化为数组。
```
console.log(Array.from([1, 2]));	//[1, 2]
console.log(Arrya.from([1, , 3]));	//[1, undefined, 3];
```

mapFn
map函数，用于对每个元素进行处理，放入数组的事处理后的元素。
```
console.log(Array.from([1, 2, 3], (n) => n *2));
//[2, 4, 6]
```

### 类数组对象
一个类数组对象必须含有length属性，且元素属性名必须是数值或者可转换为数值的字符。
```
let arr = Array.from({
  0: '1',
  1: '2',
  2: 3,
  length: 3
})
arr;  //['1', '2', 3]

//没有length属性，则返回空数组
let arr1 = Array.from({
  0: '1',
  1: '2',
  2: 3,
})
arr1;  //[]
//元素属性名不为数值且无法转换为数值，返回长度为length元素值为undefined的数组
```

### 转换可迭代对象
- 转换map

```
let map = new Map();
map.set('key0', 'value0');
map.set('key1', 'value1');
console.log(Array.from(map)); // [['key0', 'value0'],['key1', 'value1']]
```

- 转换set

```
let arr = [1, 2, 3];
let set = new Set(arr);
console.log(Array.from(set)); // [1, 2, 3]
```

- 转换字符串

```
let str = 'abc';
console.log(Array.from(str)); //["a", "b", "c"]
```

### 扩展的方法
- find()

查找数组中符合条件的元素，若有多个符合条件的元素，则返回第一个元素。
```
let arr = Array.of(1, 2, 3, 4);
console.log(arr.find(item => item > 2)); //3

console.log([, 1].findIndex(n => true)); //undefined
```

- findIndex()

查找数组中符合条件的元素索引，若有多个符合条件的元素，则返回第一个元素索引。
```
let arr = Array.of(1, 2, 1, 3);

console.log(arr.findIndex(item => item = 1)); // 0

console.log([, 1].findIndex(n => true)); //0
```

- fill()

将一定范围索引的数组元素内容填充为单个指定的值。
```
let arr = Array.of(1, 2, 3, 4);
//参数1：用来填充的值
//参数2：被填充的起始索引
//参数3：被填充的结束索引，默认为数组末尾
console.log(arr.fill(0,1,2)); //[1, 0, 3, 4]
```

- copyWithin()

将一定范围索引的数组元素修改为此数组另一指定范围索引的元素。
参数1：被修改的起始索引，负数表示倒数。
参数2：被用来覆盖的数据的起始索引；
参数3：被用来覆盖的数据的结束索引，默认为数组末尾。

- entries()

遍历键值对。
```
for (let [key, value] of ['a', 'b'].entries()){
	console.log(key, value);
}

//不使用循环
let entries = ['a', 'b'].entries();
console.log(entries.next().value); //[0, "a"]
console.log(entries.next().value); // [1, "b"]
```

- keys()

遍历键名。
```
console.log([...[,'a'].keys()]); // [0, 1]
```

- values()

遍历键值。
```
console.log([...[,'a'].values()]); // [undefined, "a"]
```

- includes()

数组是否包含指定值。
与Set和Map的has方法区分：
Set的has方法用于查找值；
Map的has方法用于查找键名。
```
[1, 2, 3].incluede(1);	//true
```

### 扩展运算符
复制数组
```
let arr = [1, 2],
	arr1 = [...arr];
console.log(arr1); //[1, 2]

//合并数组
console.log([..[1, 2], ...[3,4]]); //[1, 2, 3, 4]
```

## ES6函数

### 箭头函数
箭头函数提供了一种更加简洁的函数书写方式。基本语法是：
参数 => 函数体

基本用法：
```
var f = v => v;
//等价于
var f = function(a){
 return a;
}

f(1); //1
```

>当箭头函数没有参数或者有多个参数，要用`()`括起来。
>当箭头函数体有多行语句，用`{}`包裹起来，表示代码块。
>当箭头函数要返回对象的时候，为了区分于代码块，要用`()`将对象包裹起来。
>没有this、super、arguments和new.target绑定。
>箭头函数中的this对象，是定义函数时的对象，而不是使用函数时的对象。
>不可以违构造函数，也就是不能使用new命令，否则会报错。

## 迭代器
iterator是ES6引入的一种新的遍历机制，迭代器有两个核心概念：

- 迭代器是一个统一的接口，它的作用是使用各种数据结构可被便捷的访问，他是用过一个键为Symbol.iterator的方法来实现。
- 迭代器是用于遍历数据结构元素的指针（如数据库中的游标）。

### 迭代过程
迭代的过程如下：

- 通过Symbol.iterator创建一个迭代器，指向当前数据结构的起始位置；
- 随后通过next放下进行向下迭代指向下一个位置，next方法会返回当前位置的对象，对象包含了value和done两个属性。value是当前属性的值，done用于判断是否遍历结束；
- 当done为true时，遍历结束。

```
const items = ["zero", "one", "two"];
const it = items[Symbol.iterator]();

it.next();
//{value: "zero", done: false}
it.next();
//{value: "one", done: false}
it.next();
//{value: "two", done: false}
it.next();
//{value: undefined, done: true}
```

可迭代的数据结构：

- Array
- String
- Map
- Set
- Dom元素（正在进行中）

普通对象不可迭代，普通对象是由object创建的。

### for...of循环
for...of是ES6新引入的循环，用于替代for...in和forEach()，并支持新的迭代协议。它可用于迭代常规的数据类型，如Array、String、Map和Set等等。
Array
```
const nums = ["zero", "one", "two"];

for (let num of nums) {
  console.log(num);
}
TypedArray
const typedArray1 = new Int8Array(6);
typedArray1[0] = 10;
typedArray1[1] = 11;

for (let item of typedArray1) {
  console.log(item);
}
```

of操作数必须是可迭代，这意味着如果是普通对象则无法进行迭代。如果数据结构类似于数组的形式，则可以借助Array.from()方法进行转换迭代。

如果使用let和const，每次迭代将会创建一个新的存储空间，这可以保证作用于在迭代的内部。
var会作用于全局，迭代将不会每次都创建一个新的存储空间。
## Class类
在ES6中，class（类）作为对象的模板被引入，可以通过class关键字定义类。
class的本质是function。
它可以看做一个语法糖，让对象原型的写法更加清晰、更像面向对象编程的语法。

### 基础用法
类定义
类表达式可以为匿名或命名。

```
//匿名类
let Example = class {
	constructor(a) {
		this.a = a;
	}
}
//命名类
let Example = class {
	constrctor(a) {
		this.a = a;
	}
}
```

类声明
```
class Example {
	constructor(a) {
		this.a = a;
	}
}
```

### 类的主体
属性prototype
静态属性：class本身的属性，直接定义在类内部的属性，不需要实例化。
```
Example.a = 2;
```

公共属性：
```
Example.prototype.a = 2;
```

实例属性：定义在实例对象（this）上的属性。
```
class Example {
	a = 2;
	constructor() {
		console.log(this.a)
	}
}
```

name属性：返回跟在class后的类名（命名类）。
```
let Example = class Exam {
	constructor(a) {
		this.a = a
	}
}
console.log(Example.name);	//Exam
```

constructor方法
```
class Example{
	constructor(){
	  console.log('输出constructor');
	}
}
new Example();	//constuctor
```

返回对象
```
class Test {
	constructor(){
		//默认返回实例对象this
	}
}
console.log(new Test() instanceof Test);	//true

class Example {
	constructor(){
		//返回指定对象
		return new Test();
	}
}
console.log(new Example() instanceof Example);	//false
```

静态方法
```
class Example{
	static sum(a, b) {
		console.log(a+b);
	}
}
Example.sum(1, 2); //3
```

原型方法
```
class Example {
    sum(a, b) {
        console.log(a + b);
    }
}
let exam = new Example();
exam.sum(1, 2); // 3
```

实例方法
```
class Example {
    constructor() {
        this.sum = (a, b) => {
            console.log(a + b);
        }
    }
}
```

### 类的实例化
new：类的实例化必须通过new关键字。

### 封装与继承

getter与setter必须同级出现

通过extends实现类的继承。

子类constructor方法或只能怪必须有super，且必须出现在this之前。

调用父类构造函数，只能出现子类的构造函数。

调用父类方法，super作为对象，在普通方法中，指向父类的原型对象，在静态方法中，指向父类。

## ES6模块
ES6引入了模块化，其设计思想是在编译时就能确定模块的依赖关系，以及输入和输出的变量。
ES6的模块分为导出与导入两个模块。

### export与import
模块导入导出各种类型的变量，如字符串、数值、函数、类。

- 导出的函数声明和类声明必须要有名称（export default命令另外考虑）。
- 不仅能导出声明还能导出引用（例如函数）。
- export命令可以出现在模块的任何位置，但必须处于模块顶层。
- import命令会提升到模块的头部，首先执行。

### export default

- 在一个文件或模块中，export、import可以有多个，export default仅有一个。
- export default中的default是对象的导出接口变量。
- 通过export方式导出，在导入时要加{}，export default则不需要。
- export default向外暴露的成员，可是使用任意变量来接收。

