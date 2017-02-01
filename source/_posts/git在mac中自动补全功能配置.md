---
title: git在mac中自动补全功能配置
date: 2017-01-25 10:47:34
tags: [git配置,前端笔记]
categories: [Git]
---
### ps: 在 __MAC__ 中使用自带的命令行工具来操作git很不方便，没有提示对于我这种英语渣渣的人来说简直是不能接受。
***
## 话不多说直接走起

- 首先使用 `$ brew list` 命令来查看你是否已经安装了
***bash-completion*** ，如果没有继续向下进行，有的跳过下一步。
- 接下来安装 ***bash-completion*** ，执行以下命令：

  `$ brew install bash-completion`

  安装完成之后运行 `$ brew info bash-completion`

  在最下方会出现
      ==> Caveats
      Add the following lines to your ~/.bash_profile:
      if [ -f $(brew --prefix)/etc/bash_completion ]; then
      . $(brew --prefix)/etc/bash_completion
      fi
  这个是已经配置好的，正常的是没有if...then...这段的。

  这时候我们需要在当前用户根目录（即 __~/__ 目录）下创建一个 .bash_profile 文件（即创建路径为 `~/.bash_profile` ），
  接下来将

      if [ -f $(brew --prefix)/etc/bash_completion ]; then
      . $(brew --prefix)/etc/bash_completion
      fi

  添加到 .bash_profile 文件内。

  ### 注意：最好不要直接复制。如果复制的话，代码中的换行位置需要重新输入。否则最后会报错！

  安装完 ***bash-completion*** 重启终端命令行工具，继续一下步骤。

- 然后将  __Git__ 源码中的 __completion.bash__ 源码clone到本地

  `$ git clone  https://github.com/markgandolfo/git-bash-completion`

- 在下载的git-bash-completion文件夹中，找到git-bash-completion文件夹中的git-bash-completion文件。将此文件重名为.git-bash-completion（目的是将此文件隐藏在用户跟目录中，防止意外将其删除）。如果重命名不成功，可以通过 `$ vi .git-bash-completion`
   命令在用户跟目录下进行创建，然后通过编辑器将git-bash-completion文件中的内容复制到.git-bash-completion文件中。
- 之后，在用户跟目录下创建.bashrc文件，此文件也可以通过 `$ vi .bashrc` 命令进行创建，其内容如下：

        source ~/.git-completion.bashrc

 然后保存。
## 配置都已完成，重启终端命令行工具即可。
