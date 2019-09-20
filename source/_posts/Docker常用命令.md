---
title: Docker常用命令
date: 2019-09-20 11:12:26
tags: [Docker]
categories: Docker
---
## Docker常用命令
> 接下来主要讲述 Docker 使用已有镜像生成容器并运行，及一些相关常用命令

### 运行容器
> 在新容器中运行命令（如果本地不存在当前需要运行的容器镜像，docker 会先 pull 到本地然后运行对应镜像的容器）

命令格式
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Options:
  -a, --attach=[] 登录容器（以docker run -d启动的容器）
  -c, --cpu-shares=0 设置容器CPU权重，在CPU共享场景使用
      --cap-add=[] 添加权限，权限清单详见：http://linux.die.net/man/7/capabilities
      --cap-drop=[] 删除权限，权限清单详见：http://linux.die.net/man/7/capabilities
      --cidfile="" 运行容器后，在指定文件中写入容器PID值，一种典型的监控系统用法
      --cpuset="" 设置容器可以使用哪些CPU，此参数可以用来容器独占CPU
  -d, --detach=false 指定容器运行于前台还是后台
      --device=[] 添加主机设备给容器，相当于设备直通
      --dns=[] 指定容器的dns服务器
      --dns-search=[] 指定容器的dns搜索域名，写入到容器的/etc/resolv.conf文件
  -e, --env=[] 指定环境变量，容器中可以使用该环境变量
      --entrypoint="" 覆盖image的入口点
      --env-file=[] 指定环境变量文件，文件格式为每行一个环境变量
      --expose=[] 指定容器暴露的端口，即修改镜像的暴露端口
  -h, --hostname="" 指定容器的主机名
  -i, --interactive=false 打开STDIN，用于控制台交互
      --link=[] 指定容器间的关联，使用其他容器的IP、env等信息
      --lxc-conf=[] 指定容器的配置文件，只有在指定--exec-driver=lxc时使用
  -m, --memory="" 指定容器的内存上限
      --name="" 指定容器名字，后续可以通过名字进行容器管理，links特性需要使用名字
      --net="bridge" 容器网络设置，待详述
  -P, --publish-all=false 指定容器暴露的端口，待详述
  -p, --publish=[] 指定容器暴露的端口，待详述
      --privileged=false 指定容器是否为特权容器，特权容器拥有所有的capabilities
      --restart="" 指定容器停止后的重启策略，待详述
      --rm=false 指定容器停止后自动删除容器(不支持以docker run -d启动的容器)
      --sig-proxy=true 设置由代理接受并处理信号，但是SIGCHLD、SIGSTOP和SIGKILL不能被代理
  -t, --tty=false 分配tty设备，该可以支持终端登录
  -u, --user="" 指定容器的用户
  -v, --volume=[] 给容器挂载存储卷，挂载到容器的某个目录
      --volumes-from=[] 给容器挂载其他容器上的卷，挂载到容器的某个目录
  -w, --workdir="" 指定容器的工作目录
```
<!-- more-->
详细讲解
```
# 端口暴露

-P参数 // docker自动映射暴露端口

docker run -d -P glud123/webapp // docker自动在host上打开49000到49900的端口，映射到容器（由镜像指定，或者--expose参数指定）的暴露端口

-p参数 // 指定端口或IP进行映射

docker run -d -p 5000:80 glud123/webapp // host上5000号端口，映射到容器暴露的80端口
docker run -d -p 127.0.0.1:5000:80 glud123/webapp // host上127.0.0.1:5000号端口，映射到容器暴露的80端口
docker run -d -p 127.0.0.1::5000 glud123/webapp // host上127.0.0.1:随机端口，映射到容器暴露的80端口
docker run -d -p 127.0.0.1:5000:5000/udp glud123/webapp // 绑定udp端口

# 网络配置

--net=bridge // 使用docker daemon指定的网桥
--net=host // 容器使用主机的网络
--net=container:NAME_or_ID // 使用其他容器的网路，共享IP和PORT等网络资源
--net=none // 容器使用自己的网络（类似--net=bridge），但是不进行配置
```

### 停止运行中的容器
> 停止一个或多个正在运行的容器

命令格式
```
docker stop [OPTIONS] CONTAINER [CONTAINER...]

Options:
  -t, --time int   在结束当前容器之前等待停止的秒数 (默认 10 秒)
```

### 启动已经停止的容器
> 启动一个或多个已经停止的容器

命令格式
```
docker start [OPTIONS] CONTAINER [CONTAINER...]

Options:
  -a, --attach               Attach STDOUT/STDERR and forward signals
      --detach-keys string   Override the key sequence for detaching a container
  -i, --interactive          Attach container's STDIN
```

### 重启运行中的容器
> 重启一个或多个正在运行的容器

命令格式
```
docker restart [OPTIONS] CONTAINER [CONTAINER...]

Options:
  -t, --time int   在结束当前容器之前等待停止的秒数 (默认 10 秒)
```

### 进入一个运行中的容器
> 将本地标准输入、输出和错误流附加到正在运行的容器

命令格式
```
docker attach [OPTIONS] CONTAINER

Options:
      --detach-keys string   Override the key sequence for detaching a container
      --no-stdin             Do not attach STDIN
      --sig-proxy            Proxy all received signals to the process (default true)
```

### 显示正在运行的容器
> 容器清单

命令格式
```
docker ps [OPTIONS]

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
```

### 显示本地镜像
> 镜像清单

命令格式
```
docker images [OPTIONS] [REPOSITORY[:TAG]]

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs
```

### 删除本地镜像
> 删除一个或多个镜像

命令格式
```
docker rmi [OPTIONS] IMAGE [IMAGE...]

Options:
  -f, --force      Force removal of the image
      --no-prune   Do not delete untagged parents
```

### 删除容器
> 删除一个或多个容器（只能删除未运行的容器）

命令格式
```
docker rm [OPTIONS] CONTAINER [CONTAINER...] 

Options:
  -f, --force     Force the removal of a running container (uses SIGKILL)
  -l, --link      Remove the specified link
  -v, --volumes   Remove the volumes associated with the container
```

### 查看镜像历史
> 展示镜像的历史记录

命令格式
```
docker history [OPTIONS] IMAGE

Options:
      --format string   Pretty-print images using a Go template
  -H, --human           Print sizes and dates in human readable format (default true)
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs
```

### 构建镜像
> 从 Dockerfile 文件构建镜像

命令格式
```
docker build [OPTIONS] PATH | URL | -

Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is 'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN instructions during build (default "default")
      --no-cache                Do not use cache when building the image
      --pull                    Always attempt to pull a newer version of the image
  -q, --quiet                   Suppress the build output and print image ID on success
      --rm                      Remove intermediate containers after a successful build (default true)
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
  -t, --tag list                Name and optionally a tag in the 'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])
```

### 导出容器
> 将容器的文件系统导出为tar存档

命令格式
```
docker export [OPTIONS] CONTAINER

Options:
  -o, --output string   Write to a file, instead of STDOUT
```

### 保存镜像
> 将一个或多个图像保存到tar存档（默认情况下流式传输到 STDOUT）

命令格式
```
docker save [OPTIONS] IMAGE [IMAGE...]

Options:
  -o, --output string   Write to a file, instead of STDOUT
```

### 加载镜像
> 从 tar 存档或 STDIN 加载镜像

命令格式
```
docker load [OPTIONS]

Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output
```
