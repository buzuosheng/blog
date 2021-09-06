---
title: 从minipack看打包原理
date: 2020-06-23 21:10:44
tags: webpack
categories: webpack
---

# 从minipack看打包原理

前端有很多的打包工具如webpack等，但是打包工具的原理是什么呢？

[minipack](https://github.com/ronami/minipack/)是一个小型的打包工具，作者ronami，用来解析打包工具的基本原理。代码中有相当多的注释，理解起来也非常容易。

先放其中测试的代码文件，代码文件只有三个，每个都只有一两行代码：

``` js
// entry.js
import message from './message.js';

console.log(message);

// message.js
import {name} from './name.js';

export default `hello ${name}!`;

// name.js
export const name = 'world';
```

> 在模块化编程中，开发人员将程序分解为离散的功能块，称为*模块*。

三个代码文件就是三个模块，它们之间存在依赖关系。

然后是正文，这是实现打包功能用到的库：

``` js
// fs读取文件
const fs = require('fs');
// path库解析文件路径
const path = require('path');
// babylon用于AST解析，构造AST语法树
const babylon = require('babylon');
// travers对AST语法树进行遍历
const traverse = require('babel-traverse').default;
// transformFromAst将语法树转换为代码
const {transformFromAst} = require('babel-core');
```

## createAsset获取模块信息

第一个函数是createAsset()函数，获取模块的信息。主要包括四个部分：

- id：每个模块的唯一标识符；
- filename：模块的文件名；
- dependencies：模块的依赖列表，数据结构为数组；
- code：模块中的代码。

``` js
let ID = 0;

// 接受一个文件参数，为模块创建一个抽象语法树，
// 遍历该树，得到模块的信息对象，属性包括id,文件名，依赖，代码
function createAsset(filename) {
  // 获取文件内容，编码格式utf-8
  const content = fs.readFileSync(filename, 'utf-8');

  // JavaScript解析器会生成抽象语法树
  const ast = babylon.parse(content, {
    sourceType: 'module',
  });

  const dependencies = [];
  // 遍历抽象语法树，从导入声明中获取依赖列表
  traverse(ast, {
    ImportDeclaration: ({node}) => {
      dependencies.push(node.source.value);
    },
  });

  // 获取id
  const id = ID++;

  // 从AST解析获取代码
  const {code} = transformFromAst(ast, null, {
    presets: ['env'],
  });

  return {
    id,
    filename,
    dependencies,
    code,
  };
}
```

如entry.js文件经过该函数处理后，会得到以下数据：

``` js
{
  id: 0,
  filename: './example/entry.js',
  dependencies: ['./message.js'],
  code: '"use strict";\n' +
    '\n' +
    'var _message = require("./message.js");\n' +
    '\n' +
    'var _message2 = _interopRequireDefault(_message);\n' +
    '\n' +
    'function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }\n' +
    '\n' +
    'console.log(_message2.default);'
}
```

## 构建依赖图

第二个函数createGraph()接受一个文件，从该文件开始向前遍历，直到处理完所有的模块。

``` js
// 函数接受一个入口文件，从入口文件开始，向前递归寻找依赖文件，最后返回一个包含所有模块的数组
function createGraph(entry) {
  // 从入口文件开始获取依赖项
  const mainAsset = createAsset(entry);

  // 创建一个数组类型的队列，起始队列中只有入口文件一个元素
  const queue = [mainAsset];
  // 使用for..of...循环遍历队列，添加一个mapping对象，将依赖项的相对地址改为绝对地址
  for (const asset of queue) {
    asset.mapping = {};

    const dirname = path.dirname(asset.filename);
    // 将每个依赖项的相对地址转为绝对地址，获取到依赖项的全部信息后，
    asset.dependencises.forEach(relativePath => {
      const absolutePath = path.join(dirname, relativePath);

      const child = createAsset(absolutePath);

      // 为mapping添加relativePath属性，属性值为依赖项的id
      asset.mapping[relativePath] = child.id;
      // 将child推到依赖项队列中
      queue.push(child);
    });
  }

  return queue;
}
```

在第二个函数中，为模块对象添加了一个mapping属性，用于保存依赖的模块的相对路径和模块id的映射。如{'./message.js': 1}。遍历完成后，函数返回一个依赖图数组。返回结果大致如下：

``` js
[
  {
    id: 0,
    filename: './example/entry.js',
    dependencies: [ './message.js' ],
    code: '',
    mapping: { './message.js': 1 }
  },
  {
    id: 1,
    filename: 'example/message.js',
    dependencies: [ './name.js' ],
    code: '',
    mapping: { './name.js': 2 }
  },
  {
    id: 2,
    filename: 'example/name.js',
    dependencies: [],
    code: '',
    mapping: {}
  }
]
```

mapping属性解决的问题是，当依赖列表中出现相同的文件时，可以使用唯一标识符id进行区分。

## bundle函数

bundle()函数首先对参数进行处理，对每一个模块进行处理，将所有的模块转换成key:value形式，key为模块的唯一标识符id，value是一个二值数组，第一个值是模块的代码，第二个值是mapping。代码如下：

``` js
function bundle(graph) {

  let modules = '';

  graph.forEach(mod => {

    modules += `${mod.id}: [
      function (require, module, exports) {
        ${mod.code}
      },
      ${JSON.stringify(mod.mapping)},
    ],`;
  });

  const result = `
    (function(modules) {
        function require(id) {
          const [fn, mapping] = modules[id];

          function localRequire(name) {
            return require(mapping[name]);
          }

          const module = { exports : {} };

          fn(localRequire, module, module.exports);

          return module.exports;
        }

		require(0);
      })({${modules}})
    `;
  return result;
}

const graph = createGraph('./example/entry.js');
const result = bundle(graph);

console.log(result);
```

中间这段代码与webpack的runtime函数很像。Runtime函数帮助模块顺利地执行模块的导入、导出和执行。

这里的runtime函数，接受依赖图作为参数，但是数据结构已经不同。runtime定义了一个require函数，运行require(0)，表示从入口文件开始解析，由于id具有唯一性，所以将id作为参数。

为了实现**模块化**，runtime构造了函数作用域，模块内的代码被包裹在函数内。minipack项目运行结果中modules为：

``` js
{0: [
      function (require, module, exports) {
        "use strict";

		var _message = require("./message.js");

		var _message2 = _interopRequireDefault(_message);

		function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

		console.log(_message2.default);
      },
      {"./message.js":1},
    ],
 1: [
      function (require, module, exports) {
        "use strict";

		Object.defineProperty(exports, "__esModule", {
  			value: true
		});

		var _name = require("./name.js");

		exports.default = "hello " + _name.name + "!";
      },
      {"./name.js":2},
    ],
 2: [
      function (require, module, exports) {
        "use strict";

		Object.defineProperty(exports, "__esModule", {
  		value: true
		});
		var name = exports.name = 'world';
      },
      {},
    ],}
```

## 总结

打包的基本过程为：

- 从entry开始生成AST，从导入声明中获取依赖列表
- 获取entry模块的全部信息
- 对entry的依赖文件重复上述操作，直到遍历完成
- 生成依赖图数组
- 构建runtime函数
- 将依赖图传递给runtime函数，生成代码

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)