---
layout: post
title: macOS上安装Homebrew和GCC的攻略
category: 技巧与工具
tags: GCC
keywords: GCC,Homebrew
---


> GCC（GNU Compiler Collection，GNU编译器套件），是由 GNU 开发的编程语言编译器。它是以GPL许可证所发行的自由软件，也是 GNU计划的关键部分。GCC原本作为GNU操作系统的官方编译器，现已被大多数类Unix操作系统采纳为标准的编译器，关于GCC的更多信息还可以访问它的[网站](http://gcc.gnu.org/)。

如果你在OS X上安装了Xcode，那么你是可以在Terminal下使用gcc命令来编译程序的，这似乎表明你已经安装了GCC，但事实并非如此！其实控制台只是把编译任务映射到Apple 的 clang。所以当你使用gcc命令时，注意我们没有给出要编译的文件名，所以系统报了一个错误，而错误提示写的是 clang error，可见系统确实调用的是 clang 来执行编译任务。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc01.png" width="350">
</p>

而 clang 对于很多GCC下面的组件并不支持。事实上，因为Apple为了发展自己的产品，在Xcode 7上就已经不再提供用于并行编程的 OpenMP 了（我们会在后续的文章里再来介绍在OS X下利用GCC进行基于OpenMP的并行程序开发的基本方法）。

下面我们就来详细演示在OS X系统上安装配置GCC的基本步骤，下面的测试都是在OS X 10.11 (El Capitan)系统上完成的，但是我们相信这些方法对于其他版本的OS X系统也是可行的。

作为准备工作，你需要确保你OS X系统上已经成功安装了 XCode Command Line Tools，有时在安装Xcode时，命令行工具会被一同安装，但有时并不会（这很大程度取决于你所安装的XCode的版本）。Anyway，你可以使用下面的命令来安装 XCode Command Line Tools（因为这并不是本文的重点，如果对于这一步你有所困惑，请参阅其他相关资料）

```
> xcode-select --install  
```

如果你的计算机上已经安装了XCode Command Line Tools，那么输入上述命令之后，你会得到的提示将会是像下面这样的：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc02.png" width="350">
</p>

接下来，你需要安装Homebrew，Homebrew是OS X下的一个套件管理器，用于下载、安装、维护各种软件包及其依赖包，类似Red hat下的yum，Ubuntu下的apt-get，或者你也可以理解为Python的pip工具。关于Homebrew的更多内容，你还可以范围它的[主页](http://brew.sh/index_zh-cn.html)。

Homebrew的安装方法非常简单，只需在Terminal下输入下列命令即可：

```
> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
```

安装过程可能需要几分钟时间，你所看到的安装过程中的控制台窗口应该是像下面这样的：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc03.png" width="580">
</p>

中间过程中会有一步提示你按下RETURN（Mac下的回车键）以继续，然后系统还会提示你输入管理员密码（注意下图中红线标识的部分）：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc04.png" width="580">
</p>

当出现下面这个界面时表示Homebrew已经被成功地安装了！

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc05.png" width="580">
</p>

下面就可以正式进入GCC的安装了。首先在控制台上输入

```
> brew search gcc5  
```

这是为了得到一个指定版本的GCC的安装包，我所得的结果是homebrew/versions/gcc5，所以执行安装的命令，应该采用下面这个形式：

```
> brew install homebrew/versions/gcc5  
```

执行上述命令的界面如下，表示安装程序已经启动：

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc06.png" width="580">
</p>

安装过程会持续几分钟，当出现下面这个界面时表示GCC已经被成功安装了。这时你需要得到一个你所安装的编译器的准确名字，因为你在执行编译任务时需要使用它来作为一个命令，注意：为此，你需要在控制台上输入：

```
$ g++  
```

其中`$`是命令提示符，注意输入完以上指令后，你需要按“两次”tab键，你会看到类似下图中之结果。然后你也可以输入：

```
$ g++-5 --version  
```

来获取所安装的GCC的一些版本信息。

<p align="center">
<img src="https://fzuo.github.io/assets/img/mac_gcc/mac_gcc07.png" width="580">
</p>

如此一来，你就已经成功地在OS X系统上安装了GCC编译器了，在控制台上用gcc命令并配合makefile命令的使用就可以进行编程开发了。

### 参考文献：

[网址链接](http://mathcancer.blogspot.com.au/2016/01/PrepOSXForCoding-Homebrew.html)


<span style="color:blue">**（本文完）**</span>
