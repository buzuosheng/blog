---
title: 使用webpack和rollup打包应用
date: 2021-02-23 22:02:24
tags: ['前端', '打包', '打包工具', '打包', 'webpack', '发包', 'React', 'rollup', '打包组件库','前端模块化','npm']
description: 使用webpack和rollup打包React组件库并发布到npm
categories: 前端
---

## 前言

之前做了一个loading的样式组件，为了实现代码的可重用性，将这个小项目打包并且发布在了npm上。在一次次的打包发包过程中经历了一个有一个报错，`@buzuosheng/loading`这个组件已经到了2.7.0版本，虽然还有一些要调整的地方，但总算是可以用了。

![包体的大小](image-20210224001139792.png)

## webpack和rollup对比

webpack算是使用程序员使用最多的打包工具，面试中往往会问到webpack的相关问题，而rollup被问到的要少很多。导致这种现象的一个原因是，**应用开发使用webpack，库开发使用rollup**的说法。

但是两个打包工具都有很强大的插件开发功能，功能差异越来越模糊，但是rollup使用起来更加简洁，而且能打出能小体积的文件。但当我们做前端应用时，性能分析往往要求更小的库，所以rollup更符合开发库的要求。

这次算是一个打包的实验，我们使用两个工具都对这个项目打一次包。

## 使用webpack打包

在打包之前，需要给`package.json`文件中添加或更改一些字段。

``` json
{
  // 程序主入口模块，用户引用的就是该模块的导出
  "main": "dist/bundle.js",
  // 项目包含的文件
  "files": [
    "src",
    "dist"
  ],
  // 将react和react-dom移动到该配置中，兼容依赖
  "peerDependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  },
}
```

webpack打包需要用到很多库来处理不同的文件，这个项目比较小，就只用了两个库。

``` json
// webpack.config.js
const path = require('path');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: 'production',
  entry: './src/Loading.jsx',
  output: {
    filename: "index.js",
    path: path.join(__dirname, "./dist/"),
    libraryTarget: 'umd',
  },
  optimization: {
    minimize: false,
  },
  resolve: {
    extensions: ['.jsx']
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        loader: [MiniCssExtractPlugin.loader, 'css-loader?modules'],
      },
      {
        test: /\.(js|jsx)$/,
        loader: "babel-loader",
        exclude: /node_modules/,
      },
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: "main.min.css" // 提取后的css的文件名
    })
  ],
}
```

本来应该写**开发和生产**两个环境下的配置，但在这里只写了`production`环境下的配置。

## 使用rollup打包

在rollup中使用的库比较多一点。

``` js
// rollup.config.js
// 解决rollup无法识别commonjs的问题
import commonjs from 'rollup-plugin-commonjs'
// babel处理es6代码的转换
import babel from 'rollup-plugin-babel'
// resolve将我们编写的源码与依赖的第三方库进行合并
import resolve from 'rollup-plugin-node-resolve'
// postcss处理css文件
import postcss from 'rollup-plugin-postcss'

export default {
  input: "src/Loading.jsx",
  // 打包一份cjs和一份es的文件
  output: [
    {
      file: "dist/loading.es.js",
      format: "es",
      globals: {
        react: 'React',
      },
    }, {
      file: 'dist/loading.cjs',
      format: "cjs",
      globals: {
        react: 'React',
      },
    },
  ],
  external: ['react'],
  plugins: [
    postcss(
      { extensions: ['.css'] }
    ),
    babel({
      exclude: "node_modules/**",
      runtimeHelpers: true,
    }),
    commonjs(),
    resolve(),
  ],
}
```

## 发包到npm

发包到npm只需要几个命令。

``` powershell
npm pack
```

对项目打包后，命令行输出压缩包的详细信息。

![npm打包输出](image-20210224215214179.png)

更新版本

``` powershell
npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease [--preid=<prerelease-id>] | from-git]
```

根据本次改动的大小选择不同的命令。

最后使用发布命令。

``` powershell
npm publish
```

然后就会收到邮件，你的包已经发布成功。