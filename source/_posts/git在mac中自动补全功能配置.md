---
title: Git在MAC中自动补全功能配置
date: 2017-01-25 10:47:34
tags: [git配置,前端笔记]
categories: [Git]
---
## Git在MAC中自动补全功能配置
首先使用 `$ brew list` 命令来查看你是否已经安装了
***bash-completion*** ，如果没有继续向下进行，有的跳过下一步。

接下来安装 ***bash-completion*** ，执行以下命令：

```bash
# 安装 bash-completion 
brew install bash-completion
# 安装完成之后运行
brew info bash-completion
```

在最下方会出现

```bash
==> Caveats
    Add the following lines to your ~/.bash_profile:
    if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
    fi
```


这个是已经配置好的，正常的是没有if...then...这段的。

这时候我们需要在当前用户根目录（即 __~/__ 目录）下创建一个 .bash_profile 文件（即创建路径为 `~/.bash_profile` ），
接下来将

```bash
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

添加到 .bash_profile 文件内。

**注意：最好不要直接复制。如果复制的话，代码中的换行位置需要重新输入。否则最后会报错！**

安装完 ***bash-completion*** 重启终端命令行工具，继续一下步骤。

<!--more-->

然后将  __Git__ 源码中的 __completion.bash__ 源码clone到本地

```bash
# 拉取 git-bash-completion 源码
git clone  https://github.com/markgandolfo/git-bash-completion
```

在下载的 git-bash-completion 文件夹中，找到 git-bash-completion 文件夹中的 git-bash-completion 文件。将此文件重名为 .git-bash-completion（目的是将此文件隐藏在用户跟目录中，防止意外将其删除）。

如果重命名不成功，可以通过

```bash
# 创建 .git-bash-completion 文件
vi .git-bash-completion
```

命令在用户跟目录下进行创建，然后通过编辑器将 git-bash-completion 文件中的内容复制到 .git-bash-completion 文件中。

在用户根目录下创建 .bashrc 文件

```bash
# 创建 .bashrc 文件
vi .bashrc
```

将以下内容添加到 .bashrc 文件中

```bash
source ~/.git-completion.bashrc
```

**最后，重启终端命令行工具即可。**