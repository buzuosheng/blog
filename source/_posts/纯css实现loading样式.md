---
title: 纯css实现loading样式
date: 2021-02-17 17:19:13
tags: ['面试', 'react','CSS','loading','加载','组件']
description: 纯CSS实现loading样式组件
categories: React
---

## 为什么使用CSS做图片

使用CSS生成svg矢量图，可以随着CSS文件一起缓存，减少请求图片的次数。在React中可以使用`svgr`将svg转换为组件，支持按需加载等功能。

使用CSS实现了几种简单的loading样式。

![1](纯css实现loading样式/1.svg)

![2](纯css实现loading样式/2.svg)

![3](纯css实现loading样式/3.svg)

![4](纯css实现loading样式/4.svg)

![5](纯css实现loading样式/5.svg)

![6](纯css实现loading样式/6.svg)

![7](纯css实现loading样式/7.svg)

![8](纯css实现loading样式/8.svg)

## CSS实现

使用`<svg>`标签直接写。

``` html
<svg
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
  width="40px"
  height="40px"
  viewBox="0 0 40 40"
  enable-background="new 0 0 40 40"
>
  <path
    opacity="0.2"
    fill="#000"
    d="M20.201,5.169c-8.254,0-14.946,6.692-14.946,14.946c0,8.255,6.692,14.946,14.946,14.946
    s14.946-6.691,14.946-14.946C35.146,11.861,28.455,5.169,20.201,5.169z M20.201,31.749c-6.425,0-11.634-5.208-11.634-11.634
    c0-6.425,5.209-11.634,11.634-11.634c6.425,0,11.633,5.209,11.633,11.634C31.834,26.541,26.626,31.749,20.201,31.749z"
  />
  <path
    fill="#1890ff"
    d="M26.013,10.047l1.654-2.866c-2.198-1.272-4.743-2.012-7.466-2.012h0v3.312h0
    C22.32,8.481,24.301,9.057,26.013,10.047z"
  >
    <animateTransform
      attributeType="xml"
      attributeName="transform"
      type="rotate"
      from="0 20 20"
      to="360 20 20"
      dur="1s"
      repeatCount="indefinite"
    />
  </path>
</svg>
```

其中`xmlns`属性声明命名空间，保证正常渲染出图片，否则在页面会直接渲染成xml格式。

## 包装成React组件

将svg直接包裹到React组件中，提取属性，便于配置该组件。

``` js
const Loading1 = ({ width, height, color }) => {
  return (
    <svg
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      width={width || "40px"}
      height={height || "40px"}
      viewBox="0 0 40 40"
      enable-background="new 0 0 40 40"
    >
      <path
        opacity="0.2"
        fill="#000"
        d="M20.201,5.169c-8.254,0-14.946,6.692-14.946,14.946c0,8.255,6.692,14.946,14.946,14.946
        s14.946-6.691,14.946-14.946C35.146,11.861,28.455,5.169,20.201,5.169z M20.201,31.749c-6.425,0-11.634-5.208-11.634-11.634
        c0-6.425,5.209-11.634,11.634-11.634c6.425,0,11.633,5.209,11.633,11.634C31.834,26.541,26.626,31.749,20.201,31.749z"
      />
      <path
        fill={color || "#1890ff"}
        d="M26.013,10.047l1.654-2.866c-2.198-1.272-4.743-2.012-7.466-2.012h0v3.312h0
        C22.32,8.481,24.301,9.057,26.013,10.047z"
      >
        <animateTransform
          attributeType="xml"
          attributeName="transform"
          type="rotate"
          from="0 20 20"
          to="360 20 20"
          dur="1s"
          repeatCount="indefinite"
        />
      </path>
    </svg>
  );
};

export default Loading1;
```

用户在使用该组件时，可以配置`width, height, color`三个属性。