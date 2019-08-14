---
title: Docker介绍
date: 2019-08-14 11:06:08
tags: [Docker]
categories: Docker
---

## Docker 介绍
- Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。

- Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

- 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app），更重要的是容器性能开销极低。

## Docker 三大核心概念

### 镜像（image）
镜像是创建 Docker 的基础，Docker 镜像类似于虚拟机镜像，可以理解为一个面向 Docker 引擎的只读模块，包含文件系统。
创建镜像的三种方法：
- 基于 `Dockerfile` 文件进行创建

> 按着 `Dockerfile` 的格式及规范编写 `Dockerfile` 文件，之后通过 `docker build` 命令创建新的镜像。

> `docker` 服务通过 `docker build`命令读取 `Dockerfile`配置项，并进行镜像的创建。 

- 基于已有容器的镜像进行创建

> 在已有容器的基础上进行修改后通过 `docker commit` 命令创建新的镜像。

- 基于本地 `Docker` 模版创建

> 例如通过 `OpenVZ` 提供的模板创建，`OpenVZ` 提供模版的[下载地址](https://wiki.openvz.org/Download/template/precreated)

### 容器（container）
`Docker` 容器类似一个轻量级的沙箱，`Docker` 利用容器来运行和隔离应用。

容器是从镜像创建的应用来运行实例，可以将其启动、停止、删除，而这些容器都是互相隔离、互不可见的。

### 仓库（Repository）
