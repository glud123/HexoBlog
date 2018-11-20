---
title: Homebrew安装
date: 2018-11-19 10:33:42
tags: [macOS,Mojove,brew]
categories: 工具
---
## Homebrew安装

[Homebrew官网](https://brew.sh/index_zh-cn)

在命令行工具中执行以下命令

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 疑难问题

**macOS 更新到 Mojove 安装 Homebrew 时，你会发现按着以上的操作安装不成功**

<!-- more -->

**报错如下：**

<image src="https://mdstatic.netlify.com/blog/brew安装1.png" />

**解决办法：**

安装 `Command Line Tools (macOS Mojave version 10.14)`

[Command Line Tools (macOS Mojave version 10.14)](https://download.developer.apple.com/Developer_Tools/Command_Line_Tools_macOS_10.14_for_Xcode_10.1/Command_Line_Tools_macOS_10.14_for_Xcode_10.1.dmg)

> 以上链接如果失效请尝试登录 [苹果开发者网站](https://developer.apple.com/download/more/) 进行下载

![image](https://mdstatic.netlify.com/blog/brew安装2.png)
