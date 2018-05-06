---
layout: post
title: 在Xcode中配置使用GLPK的详细步骤
category: 技巧与工具
tags: GLPK
keywords: Xcode,GLPK
---

> GLPK（GNU Linear Programming Kit:GNU线性编程工具）是用于建立线性规划LP和混合型整数规划MIP问题的建模语言工具包，同时可以用来对模型进行最优化求解。网上已经提供了较多的在Windows下基于Microsoft Visual Studio配置GLPK的方法，这里我们来谈谈在Mac电脑中基于Xcode配置GLPK的方法。

首先到GLPK的[官网](http://www.gnu.org/software/glpk/)上下载安装包，如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk01.png" width="500">
</p>

进入到Downloading页面，你会发现版本列表（如下所示），下载最新版本的tar压缩包，撰写本文时最新的GLPK版本号是4.60。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk02.png" width="400">
</p>

假设压缩包被下载到本机的Downloads文件夹，那么接下来打开terminal，并进入到该目录下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk03.png" width="380">
</p>

解压对应的压缩包：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk04.png" width="380">
</p>

然后调用configure程序来执行配置（配置过程会需要几分钟，请耐心等待）：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk05.png" width="380">
</p>

接下来make程序，如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk06.png" width="380">
</p>

然后用管理员权限执行安装命令，引用用到了sudo，所以会被要求输入密码：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk07.png" width="380">
</p>

现在 you should have a working GLPK library. 可以来check一下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk07.png" width="380">
</p>

我们已经完成了安装，但是这个工具箱还是不能直接用。是的，我们还需要在Xcode中进行配置。先是配置hearder path。如下图所示在搜索框里输入关键词，然后在对应的条目中Add the path "/usr/local/include"。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk09.png" width="500">
</p>

同理配置library search paths，如下图所示，Add the path "/usr/local/lib"

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk10.png" width="500">
</p>

最后配置Linker flag，如下图所示，Add the additional linker flag: "-lglpk"

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_glpk/mac_glpk11.png" width="500">
</p>

到此为止，所有的配置工作就都完成了。此后在你的project中的源文件里使用#include "glpk.h"便可轻松地应用GLPK中提供的各种功能了！GLPK的documents中提供了一些例子也非常便于读者自学入门，可以一并参考。

<span style="color:blue">**（本文完）**</span>
