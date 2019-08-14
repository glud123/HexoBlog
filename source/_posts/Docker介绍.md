---
title: Docker介绍
date: 2019-08-14 11:06:08
tags: [Docker]
categories: Docker
---

## Docker 介绍

- Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

- Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

- 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app），更重要的是容器性能开销极低。

## Docker 三大核心概念

<!-- more -->

### 镜像（image）

镜像是创建 Docker 的基础，Docker 镜像类似于虚拟机镜像，可以理解为一个面向 Docker 引擎的只读模块，包含文件系统。
创建镜像的三种方法：

- 基于 `Dockerfile` 文件进行创建

  > 按着 `Dockerfile` 的格式及规范编写 `Dockerfile` 文件，之后通过 `docker build` 命令创建新的镜像。
  >
  > Docker 服务通过 `docker build`命令读取 `Dockerfile`配置项，并进行镜像的创建。

- 基于已有容器的镜像进行创建

  > 在已有容器的基础上进行修改后通过 `docker commit` 命令创建新的镜像。

- 基于本地 Docker 模版创建

  > 例如通过 OpenVZ 提供的模板创建，OpenVZ 提供模版的[下载地址](https://wiki.openvz.org/Download/template/precreated)

### 容器（container）

Docker 容器类似一个轻量级的沙箱，Docker 利用容器来运行和隔离应用。

容器是从镜像创建的应用来运行实例，可以将其启动、停止、删除，而这些容器都是互相隔离、互不可见的。

镜像本身只读，容器在镜像启动时，Docker 会在镜像最上层创建一个可写层，镜像本身保持不变。

可以利用 `docker create` 命令创建一个容器，创建后的的容器处于停止状态，可以使用 `docker start` 命令来启动它。也可以直接利用 `docker run` 命令来直接从镜像启动运行一个容器。`docker run = docker creat + docker start`。

当利用 `docker run` 创建并启动一个容器时，Docker 服务的标准操作包括：

（1）检查本地是否存在指定的镜像，不存在就从公有仓库下载。

（2）利用镜像创建并启动一个容器。

（3）分配一个文件系统，并在只读的镜像层外面挂载一层可读写层。

（4）从宿主机配置的网桥接口中桥接一个虚拟的接口到容器中。

（5）从地址池中配置一个 IP 地址给容器。

（6）执行用户指定的应用程序。

（7）执行完毕后容器终止。

### 仓库（Repository）

仓库是存放 Docker 镜像的地方。仓库和注册服务器（Registry）还是有区别的。注册服务器是存放仓库的地方，在其中存放了很多仓库，每个仓库存放一类镜像文件。

仓库分为公有仓库和私有仓库，DockerHub 是目前最大的公有仓库。可以通过 `docker push/pull` 命令从仓库中上传和下载镜像，`docker search` 命令来搜索镜像。
