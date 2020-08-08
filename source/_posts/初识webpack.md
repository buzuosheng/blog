---
title: 初识webpack
date: 2020-05-08 23:32:59
tags:
---

> 本质上，webpack是一个现代JavaScript应用程序的*静态模块打包器（module bundler）*。当webpack处理应用程序时，它会递归的构建一个*依赖关系图*，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个bundle。

在开始前，需要先了解webpack中的**四个核心概念：入口（entry）、输出（output）、loader、插件（plugins）**。

- **entry：**应该使用哪个（哪些）模块来作为构建其内部依赖图的开始。
- **output：**输出所创建的bundles，以及这些文件的命名。
- **loader：**将所有类型的文件转换为webpack能够处理的有效模块。
- **plugins：**打包优化、压缩、重新定义环境中的变量等。

### entry points

标准用法：`entry: { [entryChunkName: string]: string | Array<string> }`

``` js
entry: {
  pageOne: './src/pageOne/index.js',
  pageTwo: './src/pageTwo/index.js',
  pageThree: './src/pageThree/index.js',
}
```

### output

output选项可以控制webpack如何向硬盘写入编译文件。

output的值是一个对象，包括`filename`和`path`。`filename`输出文件的文件名，`path`为输出目录的绝对路径。

``` js
output: {
  filename: 'bundle.js',
  path: '/home/project/public/assets',
}
```

此配置将一个`bundle.js`文件输出到`/home/project/public/assets`目录中。

### loader

loader用于对模块的源代码进行转换。

使用loader加载CSS文件：

```
npm install --save-dev css-loader
```

将Typescript转换为JavaScript：

```
npm install --save-dev ts-loader
```

在配置文件中进行配置：

``` js
// webpack.config.js
module: {
  rules: [
    { test: /\.css$/, use: 'css-loader' },
    { test: /\.ts$/, use: 'ts-loader' },
  ]
}
```

在webpack配置中也支持一种文件引入多个loader：

``` js
// webpack.config.js
module: {
  rules: [
    { test: /\.css$/, 
      use: [
        { loader: 'style-loader' },
        { loader: 'css-loader' }
        }
      ] 
    }
  ]
}
```

### 插件

由于插件可以携带参数/选项，所以在webpack配置中，向`plugins`传入`new`实例。

``` js
// webpack.config.js
const webpack = require('webpack');

const cnfig = {
  plugins: [
    new webpack.BannerPlugin('版权所有，翻版必究')
  ]
}

```

### 模块热替换HMR

模块热替换功能会在应用程序中替换、添加或删除模块，而无需加载整个页面。主要通过以下方式来显著加快开发速度：

- 保留在完全重新加载页面时丢失的应用程序状态。
- 只更新变更内容，以节省宝贵的开发时间。
- 调整样式更加快速，几乎相当于在浏览器调试器中更改样式。

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)

