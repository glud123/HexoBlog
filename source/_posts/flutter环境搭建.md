---
title: Flutter环境搭建
date: 2018-11-20 15:39:43
tags: [Flutter,环境搭建]
categories: Flutter
---
## Flutter环境搭建

> 以下内容基于 macOS Mojove (10.14.1) 系统的 Flutter 环境搭建过程

### 系统要求

操作系统：macOS（64-bit）
磁盘空间：700 MB（不包括 Xcode 和 Android Studio 的磁盘空间）

### 准备

- mac 工具 brew ([macOS Mojove 安装 brew 遇到的问题](https://glud.netlify.com/2018/11/19/brew%E5%AE%89%E8%A3%85/#more)) 
- [Xcode](https://itunes.apple.com/cn/app/xcode/id497799835?mt=12) 
- [Android Studio](https://developer.android.com/studio/) 

### 开始

在命令行中执行以下命令进行 Flutter 安装

```
git clone -b beta https://github.com/flutter/flutter.git
```

官方给出的会在当前的终端窗口暂时设置 PATH 环境变量

当然为了开发方便还是推荐设置全局的 Flutter 环境变量

<!-- more -->

### 设置 Flutter 为全局环境变量

1.打开 `.bash_profile` 文件，此文件可以在 `$HOME` 下进行查找

```
vi $HOME/.bash_profile
```

2.在 `.bash_profile` （没有则创建）添加

```
export PATH=$PATH:[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin
```

其中 `[PATH_TO_FLUTTER_GIT_DIRECTORY]` 为之前下载的 Flutter 文件夹路径

### 设置 Flutter 国内镜像

由于在国内访问 Flutter 有时可能会受到限制，Flutter 官方为中国开发者搭建了临时镜像，我们可以将如下环境变量加入到用户环境变量中（即上一个条目提到的 `.bash_profile` 文件中）

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

### 安装 Flutter 相关内容

设置完以上条目后，我们就可以运行 

```
flutter doctor
```

正常来讲你会报以下的错误，如图

> 当然没有报错才是正常的😆

![image](https://mdstatic.netlify.com/blog/flutter环境搭建1.png)

**[X]** Android toolchain 安卓需要的相关工具

- 这里提示的主要意思是，安卓 SDK 组件没有安装。

    这时候打开 Android Studio 按着提示进行开发工具的初始化即可， Android Studio 会下载相关的 SDK 并进行安装。

**[X]** iOS toolchain iOS需要的相关工具

- `libimobiledevice` 和 `ideviceinstaller` 没有安装
  
    逐条运行以下命令（下载耗时，大家请耐心等待！）

    ```
    brew update
    brew install --HEAD usbmuxd
    brew link usbmuxd
    brew install --HEAD libimobiledevice
    brew install ideviceinstaller
    ```
- `ios-deploy` 没有安装

    运行以下命令

    ```
    brew install ios-deploy
    ```

- 使用 Brew 来安装 iOS 的开发工具 （这是因为我在截图时还没有[安装 Brew](https://glud.netlify.com/2018/11/19/brew%E5%AE%89%E8%A3%85/#more) 😓）

**[✔️]** Android Studio 需要安装 Dart 和 Flutter 的插件

具体安装方式请查看 **[AndroidStudio-插件安装](https://glud.netlify.com/2018/11/20/androidstudio-%E6%8F%92%E4%BB%B6%E5%AE%89%E8%A3%85/)**

### 最后

恭喜你已经成功安装 Flutter 😊

![image](https://mdstatic.netlify.com/blog/flutter环境搭建2.png)








