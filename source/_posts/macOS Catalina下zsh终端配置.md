---
title: macOS Catalina下zsh终端配置
date: 2019-10-16 14:12:52
tags: [开发工具]
categories: 开发工具
---

# macOS Catalina下zsh终端配置

macOS Catalina 自带的终端已经将原有的 bash 变更为 zsh ，但是自带的终端可配置性和易用性相比第三方的还是差了些，所以就有了 **iTerm + zsh + oh-my-zsh** 的搭配方案，最终效果很赞。

**iTerm 效果**

<img src="https://mdstatic.netlify.com/blog/macOS Catalina下zsh终端配置1.png" alt="image" style="zoom:50%;" />

<!--more-->

**macOS 自带终端（terminal）效果**

<img src="https://mdstatic.netlify.com/blog/macOS Catalina下zsh终端配置2.png" alt="image" style="zoom:50%;" />

**VS Code 终端效果**

<img src="https://mdstatic.netlify.com/blog/macOS Catalina下zsh终端配置3.png" alt="image" style="zoom:70%;" />

## 安装 iTerm 终端应用

iTerm 终端 [下载地址](https://www.iterm2.com/downloads.html)

## 安装 oh-my-zsh 

oh-my-zsh [github地址](https://github.com/robbyrussell/oh-my-zsh)

```bash
# 安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 安装 Powerline

```bash
# 1、检测是否已经安装，若有版本信息则已安装
pip show powerline-status

# 2、将 powerline-status 安装在/usr/根目录中
pip install --user powerline-status

# 上一步若显示没有 pip,先安装pip
sudo easy_install pip
```

## 设置 Powerline 字体

```bash
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```

## 设置配色方案

iTerm 配色方案 [下载地址](https://iterm2colorschemes.com/)

```bash
# 直接下载tar.zip包(包含全部配色)
# 进入 iTerm2 -> Preferences -> Profiles->Color 
# 选择 Color Presets->import 选择解压好的目录下schemes目录中相应配色方案导入
```

## 切换 oh-my-zsh 主题

```bash
# 进入 zsh 配置文件
vi ~/.zshrc
# 将 ZSH_THEME 值改为 agnoster，ecs 退出，:wq 保存
```

## 安装插件

```bash
# ======================== 高亮插件 ========================
# 在 ~ 目录下新建 .zsh-plugins 文件夹
mkdir .zsh-plugins
cd .zsh-plugins
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
vim ~/.zshrc
# 文末添加以下配置
source ~/.zsh-pludins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

plugins=(zsh-syntax-highlighting)

# ======================== 自动补齐插件 ========================
# 进入之前创建的 .zsh-plugins 文件夹
cd ~/.zsh-plugins
# 创建 incr 文件夹
mkdir incr
# 访问下方地址下载 incr-0.2.zsh 文件
http://mimosa-pudica.net/src/incr-0.2.zsh
# 将文件放到 ~/.zsh-pludins/incr 下
cp 已经下载 incr-0.2.zsh 文件的所在路径 ~/.zsh-pludins/incr 
# 修改 zsh 配置文件
vim .zshrc
# 文末添加以下配置
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
```

## 修改各个终端中的字体及字号

**iTerm 字体配置**

```bash
# 进入 iTerm2 -> Preferences -> Profiles -> Text -> Font -> Change Font
# 选择 Meslo LG S for Powerfine, 常规， 12
```

**macOS terminal 字体配置**

```bash
# 进入 terminal -> 终端 -> 偏好设置 -> 文本 -> 字体 -> 更改字体
# 选择 所有字体 -> Meslo LG S for Powerfine, 常规， 12
```

**VS Code 字体配置**

```bash
# 进入 设置 -> 功能 -> 终端 
# Integrated: Font Family 配置项 -> Meslo LG S for Powerline
# Integrated: Font Size 配置项 -> 12
```

最后可能需要重启终端查看效果。