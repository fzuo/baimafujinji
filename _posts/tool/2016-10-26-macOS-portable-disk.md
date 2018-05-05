---
layout: post
title: macOS移动硬盘中文件无法读取
category: 技巧与工具
tags: macOS
keywords: macOS,移动硬盘
---

> 初用Mac电脑的人总会遇到这样或那样的问题。其中一个就是读写移动硬盘。Windows下的文件系统NTFS在OS X下是不能使用的！通常，如果你移动硬盘的文件系统是NTFS，那么你只能读文件，却不能写文件。

要想让你的Mac电脑能够自如地读写硬盘，其实有很多方法。最简单的方法就是装一个转换软件。此时你的选择也有很多！比较常被推荐的有下面两个：

1. Tuxera NTFS for Mac (http://www.tuxera.com/products/tuxera-ntfs-for-mac/)

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_01.png" width="200">
</p>

2. Mounty for NTFS (http://enjoygineering.com/mounty/)

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_02.png" width="200">
</p>

其中，Tuxera NTFS（或Tuxera Disk Manager）是收费软件，用起来相对容易一些，Mounty for NTFS是免费软件，其实用起来也还可以。

你装了这两个软件任意之一，都可以实现在OS X中读写NTFS文件文件的需求。

但是有时候，OS X系统读移动硬盘中文件时，提示已被OS X使用，不能打开，例如像下面这样：

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_03.png" width="280">
</p>

或者移动硬盘上的文件是灰白色，根本点不开，像下面这样：

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_04.png" width="500">
</p>

这个时候你该怎么办？不用急，非常简单。这其实是文件属性问题，在Terminal中可以看到文件多了个@属性。需要用xattr删掉@属性。

具体方法演示如下，首先鼠标右键点击你打不开的文件，在弹出的右键菜单里选择 【Copy “\*\*\*\*\*\*”】，如下图所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_05.png" width="400">
</p>

然后进入你的Terminal，在命令行输入xattr -d com.apple.FinderInfo （此处加上你之前复制的包含路径的文件名），例如下图所示，其中红线部分是我刚刚拷贝过来的（包含路径的）文件名：

<p align="center">
<img src="https://fzuo.github.io/assets/img/img/mac_disk/mac_disk_05.png" width="500">
</p>

输入命令后回车，你的问题就解决了，之前打不开的文件现在已经可以自由访问了！



<span style="color:blue">**（本文完）**</span>
