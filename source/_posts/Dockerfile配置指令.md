---
title: Dockerfile配置指令
date: 2019-09-27 11:32:28
tags: [Docker]
categories: Docker
---

## Dockerfile配置指令

指令书写格式为

```dockerfile
INSTRUCTION arguments
```

接下来介绍一下，Dockerfile都有哪些指令以及相关说明（完全参照 [Docker v19.03 文档中 Dockerfile 参考](https://docs.docker.com/engine/reference/builder/)）

### FROM - 指定基础镜像

指令格式

```dockerfile
FROM <image> [AS <name>] 
# 或者 
FROM <image>[:<tag>] [AS <name>]
# 或者
FROM <image>[@<digest>] [AS <name>]
```

指令说明

FROM 指令初始化一个新的构建阶段，并为后续指令设置基本镜像。

Dockerfile 须从 FROM 指令开始，必须是有效的镜像。

<!--more-->

### RUN - 构建镜像时执行的命令

指令格式

```dockerfile
RUN <command>
# 或者
RUN ["executable", "param1", "param2"]
```

指令说明

前者通过 shell 终端中运行命令，后者指定其他终端运行相关参数

```dockerfile
RUN /bin/sh -c "echo hello"
# 等价于
RUN ["/bin/sh","-c","echo hello"]
# 因为 RUN 指令默认使用 /bin/sh 
# 如果需要使用其他 shell 如 /bin/bash 
# 就需要使用后者书写方式
RUN ["/bin/bash","-c","echo hello"]
```

每个 RUN 指令将在当前镜像上执行命令，并提交为新的镜像。

**如果命令较长时，可以使用 `\` 来进行换行。**

### CMD - 容器运行时执行的命令

指令格式

```dockerfile
CMD ["executable","param1","param2"] # 首选方式
# 或者
CMD ["param1","param2"] # 作为ENTRYPOINT的默认参数
# 或者
CMD command param1 param2 # 在 /bin/sh 中执行
```

指令说明

CMD 用于指定容器启动时要执行的命令。(RUN 指令是在 build 时执行的命令，并提交为新的镜像)

每个 Dockerfile 只能有一个 CMD 命令，多个 CMD 命令只执行最后一个。

若在启动容器时指定了运行时的命令，则会覆盖掉 Dockerfile 中 CMD 指令中指定的命令。

### LABEL - 为镜像添加元数据

指令格式

```dockerfile
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

指令说明

将元数据添加到镜像中，以键值对方式添加，如果 LABEL 值中包含空格，需要使用双引号`""`，如果需要换行使用反斜线`\`。

```dockerfile
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

Dockerfile 中可以包含多个 LABEL ，一个 LABEL 指令可以设置多个标签

```dockerfile
# 在一行设置多个 LABEL
LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
# 等价于
# 设置多个 LABEL 通过 \ 进行换行
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```

### MAAINAINER - 作者信息（已弃用）

如果添加作者信息，官方推荐使用 LABEL 指令

```dockerfile
LABEL maintainer="SvenDowideit@home.org.au"
```

### EXPOSE - 与外界交互的端口

指令格式

```dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

指令说明

该指令告知 Docker 容器在运行时监听指定的网络端口，默认协议类型为 TCP ,也可指定 UDP 。

```dockerfile
# 指定协议为 UDP 
EXPOSE 80/udp

# 如果同时在 TCP 和 UDP 公开
EXPOSE 80/tcp
EXPOSE 80/udp

```

**无论 EXPOSE 指令如何设置，都可以在容器运行时通过 `-p ` 标志覆盖 Dockerfile 中配置的 EXPOSE 指令**

### ENV - 环境变量

指令格式

```dockerfile
ENV <key> <value>
# 或者
ENV <key>=<value> ...
```

指令说明

用于指定环境变量，这些环境变量，后续可以被RUN指令使用，容器运行起来之后，也可以在容器中获取这些环境变量。

```dockerfile
ENV myName="John Doe" myDog=Rex\ The\ Dog \
    myCat=fluffy
# 等价于
ENV myName John Doe
ENV myDog Rex The Dog
ENV myCat fluffy
```

### ADD - 将本地或者网络资源添加到镜像内

指令格式

```dockerfile
ADD [--chown=<user>:<group>] <src>... <dest>
# 或者
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"] #（此格式对于包含空格的路径是必需的）
```

指令说明

该命令将复制指定本地目录中的文件到容器中的 dest 中，src 可以是是一个绝对路径，也可以是一个URL 或一个 tar 文件，tar 文件会自动解压为目录。

```dockerfile
# 示例
ADD hom* /mydir/          # 添加所有以"hom"开头的文件
ADD hom?.txt /mydir/      # ? 替代一个单字符,例如："home.txt"
ADD test relativeDir/     # 添加 "test" 到 `WORKDIR`/relativeDir/
ADD test /absoluteDir/    # 添加 "test" 到 /absoluteDir/
```



### COPY - 功能类似 ADD，不能自动解压文件，也不能访问网络资源

指令格式

```dockerfile
ADD [--chown=<user>:<group>] <src>... <dest>
# 或者
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"] #（此格式对于包含空格的路径是必需的）
```

指令说明

复制本地主机 src 目录或文件到容器的 desc 目录，desc 不存在时会自动创建。

### ENTRYPOINT - 容器启动时执行的命令（此处可以增加易用性优化）

指令格式

```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
# 或者
ENTRYPOINT command param1 param2
```

指令说明

用于配置容器启动后执行的命令，这些命令不能被 `docker run` 提供的参数覆盖。和 CMD 指令一样，每个 Dockerfile 中只能有一个 ENTRYPOINT ，当有多个时最后一个生效。

### VOLUME - 数据持久化

指令格式

```dockerfile
VOLUME ["/data"]
```

指令说明

作用是创建在本地主机或其他容器可以挂载的数据卷，用来存放数据。

一个卷可以存在于一个或多个容器的指定目录，该目录可以绕过联合文件系统，并具有以下功能：

- 卷可以容器间共享和重用

- 容器并不一定要和其它容器共享卷

- 修改卷后会立即生效

- 对卷的修改不会对镜像产生影响

- 卷会一直存在，直到没有任何容器在使用它

### WORKDIR - 工作目录

指令格式

```dockerfile
WORKDIR /path/to/workdir
```

指令说明

为后续的 RUN CMD ENTRYPOINT 指定配置工作目录，可以使用多个 WORKDIR 指令，若后续指令用得是相对路径，则会基于之前的命令指定路径。

### USER - 指定容器运行时的用户名或 UID

指令格式

```dockerfile
USER <user>[:<group>]
# 或者
USER <UID>[:<GID>]
```

指令说明

使用 USER 指定用户后，Dockerfile 中其后的命令 RUN、CMD、ENTRYPOINT 都将使用该用户。要临时使用管理员权限可以使用 sudo。在 USER 命令之前可以使用RUN命令创建需要的用户。镜像构建完成后，通过`docker run`运行容器时，可以通过 `-u` 参数来覆盖所指定的用户。

```dockerfile
FROM microsoft/windowsservercore
# Create Windows user in the container
RUN net user /add patrick
# Set it for subsequent commands
USER patrick
```

### ARG - 构建时提供的变量

指令格式

```dockerfile
ARG <name>[=<default value>]
```

指令说明

Dockerfile 可以包含一个或多个`ARG`指令

```dockerfile
FROM busybox
ARG user1
ARG buildno
...
```

不建议使用构建时变量来传递诸如github密钥，用户凭据等机密。构建时变量值对于使用该`docker history`命令的镜像的任何用户都是可见的。

### ONBUILD - 镜像触发器

指令格式

```dockerfile
ONBUILD [INSTRUCTION]
```



指令说明

当所构建的镜像被用做其它镜像的基础镜像时，该镜像中的触发器将会被触发。

例如下面的 Dockerfile 创建了镜像 A：
``` dockerfile
ONBUILD RUN python app.py
ONBUILD ADD . /app
```

则基于镜像 A 创建新的镜像时，新的 Dockerfile 中使用 from A 指定基镜像时，会自动执行 ONBUILD 指令内容，等价于在新的要构建镜像的Dockerfile中增加了两条指令：
``` dockerfile
FROM A
ADD ./app
RUN python app.py
```

