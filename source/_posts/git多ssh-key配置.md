---
title: git多ssh-key配置
date: 2019-10-15 11:23:24
tags:git
categories: Git
---

## git多ssh-key配置

在工作过程中可能使用多个 Git 账号，比如公司内部使用的 GitLab 和私人使用的 GitHub。

如果遇到类似场景可以通过以下方式实现

- 首先生成对应 SSH-Key

  ```bash
  # 生成 gitlab 对应的 SSH-Key
  ssh-keygen -t rsa -C "youremail@email.com" -f ~/.ssh/gitlab_rsa
  # 生成 github 对应的 SSH-Key
  ssh-keygen -t rsa -C "youremail@email.com" -f ~/.ssh/github_rsa
  ```

- 在 `~/.ssh` 下创建 config 文件

  ```bash
  cd ~/.ssh
  vi config # 有则打开此文件，无则新建并打开 config
  ```

- 在 config 文件中添加以下内容

  ```bash
  # gitlab.com 
  Host gitlab.com 
  Port 22 
  HostName gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_rsa
  User username
  
  # github.com
  Host github.com
  Port 22 
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/github_rsa
  User username
  ```

  配置说明

  ```bash
  # 配置文件参数
  # Host : Host可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件（可以直接填写ip地址）
  # Port: 端口号（如果不是默认22号端口则需要指定）
  # HostName : 要登录主机的主机名（建议与Host一致）
  # IdentityFile : 指明上面 User 对应的 identityFile 路径
  # User : 登录名（如gitlab的username）
  ```

  