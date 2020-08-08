---
title: 使用now.sh部署前端项目
date: 2020-06-08 19:02:41
tags:
---

now.sh是ZEIT推出的一款全球化实时部署服务。ZEIT现在已经改名为Vercel。
网站地址：[vercel.com](vercel.com)
Vercel 是一个云平台静态站点和无服务器功能完美地与您的工作流程适合。它使开发人员可以托管Jamstack网站和Web服务，这些网站和Web服务可立即部署，自动扩展且无需监督，而无需任何配置。

### 快速部署

使用now.sh部署一个React应用。首先使用github账号登陆。可以直接从github仓库中直接导入项目。
![导入项目](https://img-blog.csdnimg.cn/20200608170244583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
点击导入项目后，选择使用github导入。
![github导入](https://img-blog.csdnimg.cn/20200608170324776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
选择需要部署的项目，如果没有可选的仓库，需要先在github中设置访问权限。
![访问权限](https://img-blog.csdnimg.cn/20200608171110699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
一般只选择自己要暴露出的仓库就可以。然后点继续、继续、继续。。直到配置命令的时候。
![配置命令](https://img-blog.csdnimg.cn/2020060817254429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
本次部署的是React项目，会直接检测到，自动选择`Create React App`，然后配置打包命令等。这里打包命令设置为`npm run build`另外两个选项默认然后点击部署。
然后就会显示你的项目已经完成创建并正在部署。
![building](https://img-blog.csdnimg.cn/20200608172850568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
喝杯水/上个厕所/透个气/随便干点什么的功夫，就已经部署好了。这时，左侧会显示出网站的预览图，右侧显示出状态信息。
![部署成功](https://img-blog.csdnimg.cn/20200608173303611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
右侧的域名中有三个地址可以访问到该页面，状态显示Ready，一切正常，点击任意域名可以访问。
网站部署地址：[wuqiku-buzuosheng.now.sh](wuqiku-buzuosheng.now.sh)

### 部署log查看

![deployments](https://img-blog.csdnimg.cn/20200608173828895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
在Deployments中可以查看该项目的部署list，点击可以查看输出的log信息，如果部署失败可以查看错误信息，改动后自动重新部署。
点开一条可以看到详细信息。
![overview](https://img-blog.csdnimg.cn/20200608185502454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
这里是点开一条部署失败的记录，类似之前的预览，但在下方的Build Logs中会输出详细的信息，可以根据报错修改自己的代码。
![Source](https://img-blog.csdnimg.cn/20200608185049363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
可以查看项目的资源文件。

### 项目设置

![设置](https://img-blog.csdnimg.cn/20200608174214812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70)
在项目的设置中可以修改域名、打包命令、根目录等。
主要修改的就是自己的域名，一般都需要为自己的网站设置一个比较容易记住的域名，而不是一串哈希值。

我的博客即将同步至腾讯云+社区，邀请大家一同入驻：https://cloud.tencent.com/developer/support-plan?invite_code=2uwbqlb5sl4wg

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)