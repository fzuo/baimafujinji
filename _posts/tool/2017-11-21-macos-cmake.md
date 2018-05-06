---
layout: post
title: 在macOS中配置CMake的详细图文教程
category: 技巧与工具
tags: macOS
keywords: macOS,CMake
---

> CMake是一个比make更高级的跨平台的安装、编译、配置工具，可以用简单的语句来描述所有平台的安装（编译过程）。并根据不同平台、不同的编译器，生成相应的Makefile或者project文件。本文主要介绍在Mac OSX上安装配置CMake的方法，笔者所使用的OS X版本是macOS Sierra。

首先，你要去CMake的网站上下载它的安装文件（官网下载地址：https://cmake.org/download/），选择下图中红框所标注出来的选项。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake01.png" width="500">
</p>

下载完成后，就像在OS X下安装一般的程序那样安装它。完成后，CMake的图标就会出现在你的Launchpad中，如下图所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake02.png" width="120">
</p>

点击上面这个图标打开CMake的GUI程序。你可能会受到如下提示，选择【Open】然后程序可以成功打开。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake03.png" width="320">
</p>

启动程序后从菜单栏中选择【Tools】，然后从下拉菜单中选择“How to Install For Command Line Use”，便会弹出一个对话框，这个对话框提示你该如何配置，从而使得在命令行中也能够直接使用CMake命令。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake04.png" width="500">
</p>

我们主要关注其中的前两种方法。首先，如果你在命令行中输入 cmake --version，你会发现系统并不认识cmake这个命令。然后使用上面图中给出的第一种方法，即在命令行中输入PATH="/Applications/CMake.app/Contents/bin":"$PATH"，如下图所示。此后，再执行跟cmake有关的命令，系统就可以正确识别它了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake05.png" width="400">
</p>

但是，这个方法只能管一时（这跟Ubuntu下的情况类似），当你关闭命令行窗口后，再重新开启一个命令行窗口，系统仍然表示无法识别cmake命令。所以我们来尝试第二种方法。在新开的命令行窗口中输入（最好是从CMake的GUI对话框上直接复制，以免输错）

```
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install
```

此时，再次打开新的命令行窗口，cmake也可以正常运行了！

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_disk/mac_cmake06.png" width="400">
</p>

<span style="color:blue">**（本文完）**</span>
