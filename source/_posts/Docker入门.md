---
title: Docker入门
date: 2020-04-27 18:51:54
tags:
---

> Docker是一个开源的容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的Linux或Windows机器上，也可以实现虚拟化。

简单来说，Docker是一种近来很流行的虚拟化技术。

## Docker VS 传统虚拟化技术

传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整的操作系统，再在该系统上运行应用进程。docker容器内的应用进程直接运行于宿主机的内核，容器内没有自己的内核，而且也没有进行硬件虚拟，因此容器要比传统虚拟机更为轻便。

![传统虚拟化技术](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMBafRicAJKBIf1iaicGUPueNBroQPvHjTYQrUUgAlB5peWtxWn8IOvHWlrXnDlySSiaDroTjsibKesWyEQ/0?wx_fmt=png)

虚拟机在Guest OS运行各种各样的程序。

![docker](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMBafRicAJKBIf1iaicGUPueNBr0nzrWmicWZrPU0sibOG6NehEpzKzeflVtgYrVsk4ImTibmTfdichgwiaUww/0?wx_fmt=png)

docker在Docker Engine运行各种各样的程序。

|   特性   |       容器       |   虚拟机   |
| :------: | :--------------: | :--------: |
|   启动   |       秒级       |   分钟级   |
| 硬盘使用 |     一般为MB     |  一般为GB  |
|   性能   |     接近原生     |    弱于    |
| 系统支持 | 单机支持上千容器 | 一般几十个 |

## Docker基本概念

Docker包括三个基本概念：镜像（images）、容器（container）、仓库（repository）。

### 镜像

**Docker镜像是一个特殊的文件系统**，包括容器运行时需要的程序、库、资源、参数等，但不包含任何动态数据，内容在构建后也不会被改变。

### 容器

**容器的实质是进程。**但与直接在宿主机执行的进程不同，容器进程运行于属于自己独立的命名空间，容器的文件系统，网络配置等都独立于宿主机的系统，这种良好的封装性使得应用比直接在宿主机运行更加安全。

**镜像与容器的关系，就像是面向对象程序设计中的类与实例的关系**，镜像是静态的定义，容器是镜像运行时的实体，。

### 仓库

Docker Registry是一个集中存储、分发镜像的服务。

一个Docker Registry中可以包含多个仓库（repository），每个仓库可以包含多个标签的镜像，不同的标签对应不同的版本。

## 安装与配置

Docker可以直接在官网下载，官网下载速度慢，国内也有很多资源可以下载，但是镜像的下载速度也很慢，这同样有解决的办法。

国内阿里云提供了镜像加速服务，需要注册一个阿里云账号，即可获取阿里云提供的个人专属加速服务。

阿里云镜像加速地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors?accounttraceid=00ade0bc8c4f403496ba8924e677f991sjzv

![阿里云镜像加速](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMBafRicAJKBIf1iaicGUPueNBruUJhQgBVb3KicS9VehiaYyiawj8acefA7ib8PkiaTyNozpgdnOabbsSnfqQ/0?wx_fmt=png)

当然也可以使用别人的加速地址，但总是自己的更好用，下边同时提供了几种不同系统的操作文档。

还有一个在线的工具Play with docker可以使用：

地址：https://labs.play-with-docker.com/

![play with docker](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMBafRicAJKBIf1iaicGUPueNBrEy8ic3reuHLfd9sn5umGpNa9FtafI6nXxpCFILcebHnFXZEUGNKas6Q/0?wx_fmt=png)

登陆自己的docker ID就可以免费使用了。

## 使用镜像

Docker Hub上已经有大量优质的镜像可供使用，我们将镜像获取到本地只需要一个命令:`docker pull nginx:latest`。`:latest`表示获取镜像的版本号，此处为最新的。

如果我们在`docker run`运行容器时，会检查是否有该镜像，如果没有会自动拉取。

常用的命令：

- `docker images`：列出所有的镜像。
- `docker rmi`：删除镜像。
- `docker build`：构建镜像。
- `docker run`：运行一个容器。

除了使用docker hub上的镜像，我们也可以定制自己的镜像。

### 构建镜像上下文

构建命令格式为：`docker build [选项] <上下文路径/url/->`。

例如：`docker build -t nginx:v1 .`，-t表示该镜像的名字及标签，最后的`.`表示当前目录。

构建镜像的时候，用户会指定构建镜像上下文的路径，`docker build`获取到这个路径后，会将路径下的所有内容打包，然后上传给docker引擎，这样docker引擎就会获得构建镜像所需要的一切文件。

当然我们也会有不想被打包进去的文件我们可以使用类似`.gitignore`文件的`.dockerignore`文件，让build命令在打包的时候，忽略一些文件。

### Dockerfile指令

**Dockerfile是一个文本文件，其中包含了一条条的指令，每一条指令构建一层，所以每一条指令描述了该层该如何构建。**

常用的Dockerfile指令：

- `FROM`：描述该镜像的基础镜像来源。
- `RUN`：用来执行命令行命令。
- `COPY`：从**构建上下文目录中原路径**的文件/目录复制到新的一层的镜像内目标路径位置。
- `ADD`：比`COPY`更高阶的复制文件，`ADD`的指令可以是一个url，也可以是压缩包。
- `CMD`：用于指定默认的容器主进程的启动命令。
- `ENTRYPOINT`：指定了`ENTRYPOINT`之后，`CMD`就不是直接运行启动命令，而是将`CMD`的内容作为参数传递给`ENTRYPOINT`指令。
- `ENV`：设置环境变量。
- `VOLUME`：定义匿名卷。
- `EXPOSE`：声明运行时容器提供服务端口。
- `WORKDIR`：用于指定工作目录。

## Docker Compose

Compose项目是Docker官方的开源项目，负责实现对Docker容器集群的快速编排。

> `Compose`的定位是“定义和运行多个Docker容器的应用Defining and running multi-container Docker applications）”，其前身是开源项目Fig。

我们可以使用`Dockerfile`模板文件定义一个单独的引用容器。但是我们通常需要多个容器相互配合来完成某项任务的情况。例如，要实现一个web项目，除了Web服务容器本身，往往还需要加上后端的数据库服务容器，甚至还包括负载匀衡容器等。

`Compose`恰好满足了这样的需求。**它允许通过一个单独的`docker-compose.yaml`模板文件来定义一组组相关联的应用容器为一个项目。**

`Compose`中有两个重要的概念：

- `service`：一个应用的容器，实际上可以包含若干运行相同镜像的容器实例。
- `project`：由一组组相关联的应用容器组成一个完整的应用单元。

### Compose模板文件

默认的模板文件名为`docker-compose.yaml`，格式为YAML格式。

模板文件中主要有version、service、networks三个部分。

每个服务都必须通过`image`指令指定镜像或`build`命令等来自动构建生成镜像。如果使用build命令，在`Dockerfile`中设置的选项将会自动被获取。

其中有几个常用的指令。

- `build`：指定`Dockerfile`所在的文件夹，`compose`将会利用它自动构建镜像，然后使用这个镜像。
- `depends_on`：解决容器的依赖、启动先后的问题。
- `environment`：设置环境变量。
- `expose`：暴露端口，但不映射到主机，只被连接的服务访问。
- `image`：指定为镜像名称或者镜像ID，如果本地不存在，`compose`将会尝试拉取这个镜像。
- `labels`：为容器添加Docker元数据信息。
- `networks`：配置容器连接的网络。
- `ports`：暴露端口信息。使用宿主端口：容器端口格式，或仅仅指定容器的端口。
- `volumes`：数据卷所挂载的路径设置。可以设置为宿主机路径或者数据卷名称，并且可以指定访问模式。

