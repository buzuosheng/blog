---
title: 'tailwindcss: 弟弟们都往后稍稍'
date: 2021-03-20 19:50:05
tags: ['css', 'tailwindcss', 'less', 'sass', 'css module', 'css-in-js']
description: 
categories: 前端
---

## CSS的现状

前端发展速度可以说是日新月异，但CSS作为前端重要的一部分，发展的有点让人捉急。

近些年来对于css出现了一些规范和框架，让开发者也能舒服的写css样式了。

BEM使用**模块名+元素名+修饰器名**，解决命名冲突。

postcss使用**工具和插件**转换CSS，可以为css选择器增加不同的**浏览器前缀**等。

css module为css加入**局部作用域**，实现了**css模块化**。

less和sass等css预处理语言，将**css扩展为一种编程语言**，增加变量，Mixin，函数等特性。

CSS-in-JS是一种**将css内嵌到js文件中的技术方案**，现在已经有很多种css-in-js库，支持**动态改变样式**等功能。

CSS原子化的思想是**将基础功能小的，单用途的css定义为一个class**，特点是**高复用性，低代码量**。

## tailwindcss

最近使用tailwindcss，感觉上手体验很不错，虽然刚开始需要一直查文档，但我仍然觉得未来可期。

tailwindcss有很多的优点，工具类优先，响应式设计，组件友好，高度自定义等。

我们可以用普通写法和tailwindcss做一下对比

``` html
<div class="chat-notification">
  <div class="chat-notification-logo-wrapper">
    <img
      class="chat-notification-logo"
      src="/img/logo.svg"
      alt="ChitChat Logo"
    />
  </div>
  <div class="chat-notification-content">
    <h4 class="chat-notification-title">ChitChat</h4>
    <p class="chat-notification-message">You have a new message!</p>
  </div>
</div>

<style>
  .chat-notification {
    display: flex;
    max-width: 24rem;
    margin: 0 auto;
    padding: 1.5rem;
    border-radius: 0.5rem;
    background-color: #fff;
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  }
  .chat-notification-logo-wrapper {
    flex-shrink: 0;
  }
  .chat-notification-logo {
    height: 3rem;
    width: 3rem;
  }
  .chat-notification-content {
    margin-left: 1.5rem;
    padding-top: 0.25rem;
  }
  .chat-notification-title {
    color: #1a202c;
    font-size: 1.25rem;
    line-height: 1.25;
  }
  .chat-notification-message {
    color: #718096;
    font-size: 1rem;
    line-height: 1.5;
  }
</style>
```

引入相关依赖，然后改成tailwindcss的样式。

``` html
<div class="max-w-sm mx-auto flex p-6 bg-white rounded-lg shadow-xl">
  <div class="flex-shrink-0">
    <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo" />
  </div>
  <div class="ml-6 pt-1">
    <h4 class="text-xl text-gray-900 leading-tight">ChitChat</h4>
    <p class="text-base text-gray-600 leading-normal">
      You have a new message!
    </p>
  </div>
</div>
```

肉眼可见的代码量少了很多，而且class不用再命令，既节省了想名字的时间，又直接将css以内联的形式直接写入，可谓是一步到位。

tailwindcss虽然需要前期频繁查文档，记住各种样式也是费时费力，但我依然觉得未来可期。

- **文档友好**。tailwindcss的文档我个人认为非常友好，代码和样式相互对照，而且还告知了自定义配置应该如何去做，几乎所有的样式都有。

![代码样式对照](image-20210321141816894.png)

![颜色对比](image-20210321141934145.png)

- **按需配置打包**。在生产环境，tailwindcss会自动删除所有未使用的css，尽可能的使css代码更小。

![css代码压缩](image-20210321193429531.png)

- **媒体查询**。在CSS处理媒体查询需要很多代码，tailwindcss中**使用断点语法实现媒体查询功能**，根据常用的设备分辨率，默认设置了5个断点。

``` html
<div class="grid grid-cols-1 sm:grid-cols-2 sm:px-8 sm:py-12 sm:gap-x-8 md:py-16"></div>
```

- **grid布局支持良好**。

- **添加自定义样式**。使用`@layer`等，将自定义的样式添加到全局基础样式。
- **自定义配置**。tailwindcss配置文件`tailwind.config.js`可以添加自定义的配置项。

```js
const colors = require('tailwindcss/colors')

module.exports = {
  theme: {
    colors: {
      gray: colors.coolGray,
      blue: colors.lightBlue,
      red: colors.rose,
      pink: colors.fuchsia,
    },
    fontFamily: {
      sans: ['Graphik', 'sans-serif'],
      serif: ['Merriweather', 'serif'],
    },
    extend: {
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      }
    }
  },
  variants: {
    extend: {
      borderColor: ['focus-visible'],
      opacity: ['disabled'],
    }
  }
}
```

## 总结

到目前，除了还需要在官网查api外，我使用tailwindcss的体验一直良好。

虽然也有一些想要害死强迫症的地方。

例如，rem的问题。tailwindcss中的`h4`代表的是`height: 1rem`，也就是说`h1`代表的是0.25rem。

`font-size`在刚开始是0.125rem增长，后边是0.75rem，在后边差更多。

![font-size](image-20210321225417361.png)

如果在项目的需求中，如果遇到很多1.3rem这种需求，就需要做大量的配置。

但我觉得也有不少贴近生活的语义化。

例如`font-size`的大小，是按照平时衣服尺码的`xl, 2xl`这种。还有`border-raduis`改为了`rounded`也不错

