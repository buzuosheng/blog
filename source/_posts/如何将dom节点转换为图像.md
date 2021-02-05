---
title: 如何将dom节点转换为图像
date: 2021-02-05 17:53:18
tags: ['dom转图片', 'dom-to-image', '前端', '面试']
description: 如何将网页DOM转换成图片
categories: 面试
---

前端经常会遇到这样的业务场景，页面生成图片用于分享活动。

那么我们如何实现页面生成图片，也就是截图的功能呢

## dom-to-image

`dom-to-image`是一个可以将任意dom节点转换为图像的js库。

安装命令：

`npm install dom-to-image`

## 使用

- 获取png格式图片base64编码的data Url并显示

``` js
import domtoimage from 'dom-to-image'

var node = document.getElementById('my-node');

domtoimage.toPng(node)
  .then(function (dataUrl) {
    var img = new Image();
    ima.src = dataUrl;
    document.body.appendChild(img);
  })
  .catch(function (error) {
    console.log(''oops, something went wrong!', error');
  })
```

- 获得png格式图片的blob对象并且下载

``` js
domtoimage.toBlob(document.getElementById('my-node'))
    .then(function (blob) {
        window.saveAs(blob, 'my-node.png');
    });
```

- 下载压缩的jpeg格式图片

``` js
domtoimage.toJpeg(document.getElementById('my-node'), { quality: 0.95 })
    .then(function (dataUrl) {
        var link = document.createElement('a');
        link.download = 'my-image-name.jpeg';
        link.href = dataUrl;
        link.click();
    });
```

- 获得svg的data Url且过滤所有的`<i>`

``` js
function filter (node) {
    return (node.tagName !== 'i');
}

domtoimage.toSvg(document.getElementById('my-node'), {filter: filter})
    .then(function (dataUrl) {
        /* do something */
    });
```

- 以Uint8Array的方式获取原始像素数据，每4个数组元素表示一个像素的RGBA数据

``` js
var node = document.getElementById('my-node');

domtoimage.toPixelData(node)
    .then(function (pixels) {
        for (var y = 0; y < node.scrollHeight; ++y) {
          for (var x = 0; x < node.scrollWidth; ++x) {
            pixelAtXYOffset = (4 * y * node.scrollHeight) + (4 * x);
            /* pixelAtXY is a Uint8Array[4] containing RGBA values of the pixel at (x, y) in the range 0..255 */
            pixelAtXY = pixels.slice(pixelAtXYOffset, pixelAtXYOffset + 4);
          }
        }
    });
```

## 实现原理

`dom-to-image`的实现原理主要依靠**svg标签的`<foreginObject`元素和canvas**。

首先SVG要渲染成图片，必须要指定`xmlns`（命名空间），否则会直接以普通XML文档树渲染。但是如果SVG如果直接内联在XHTML中，就不能指定命名空间。而在`<foreginObject>`中允许包含任意的HTML内容，使得SVG能够正常渲染。

另外就是`canvas`绘图。

``` js
    function draw(domNode, options) {
        return toSvg(domNode, options)
            .then(util.makeImage)
            .then(util.delay(100))
            .then(function (image) {
                var canvas = newCanvas(domNode);
                canvas.getContext('2d').drawImage(image, 0, 0);
                return canvas;
            });
```

让我们来整理一下实现过程：

- 递归DOM节点，获取对应的XML代码
- 将XML放在`<foreginObject>`元素中，渲染成变成SVG图形
- 使用`canvas.drawImage()`方法将图片放在画布上
- 使用`canvas.toDataURL()`方法转换成PNG图片或JPG图片

