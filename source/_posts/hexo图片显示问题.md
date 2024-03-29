---
title: hexo图片显示问题
date: 2022-08-21 10:14:39
tags:
description:hexo博客中，本地访问需要访问asset/image路径，部署之后需要访问iamge路径
categories:
typora-root-url: hexo图片显示问题
---

在typora的设置中为图片设置一个根目录。

设置方式在上方导航栏`格式 -> 图片工具 -> 设置图片根目录`，然后选中放置图片的目录即可 

![image-20220821101859263](image-20220821101859263.png)

为了更方便一点，可以在`scaffolds -> post`中添加如下设置。

```yaml
typora-root-url: {{ title }}
```

如此在之后每次使用hexo new的时候，就会自动添加图片根目录。

这种解决方式是从本地解决问题，当插入图片(`ctrl+v`复制)的时候也会自动删除图片之前的`folder`，而只会留下图片的文件名`image.jpg`。[也不需要像之前一样删除每个图片之前的`folder`，省时省力](https://buzuosheng.com/%E5%8D%9A%E5%AE%A2/hexo%E5%8D%9A%E5%AE%A2%E5%A6%82%E4%BD%95%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87/#hexo%E4%B8%8ETypora%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88)。

![放一张壁纸图](/QQ图片20210426005912.jpg)

至此，已经能保证本地和服务端的图片都能正常预览。

但还有小小的问题，该配置有时候会变得不灵，比如新建文件之后，该配置不能立即生效，需要再点击设置或者，删掉设置再撤销一下。

同时有另一个比较大的问题，当插入图片后，在图片目录下知己添加了图片，但是文章内撤销的时候，图片目录下的图片依然存在，甚至一张图片重复多次插入撤销后，图片目录下会加入多张重复的图片。

