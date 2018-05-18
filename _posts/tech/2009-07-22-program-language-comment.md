---
layout: post
title: 十种编程语言的注释写法
category: 编程与开发
tags: Comment
keywords: Comment Syntax,编程语言
---

### 1. Java

Java中注释的写法与C++非常类似（如果不考虑Java Doc的话），

```java
// 注释一行

/* ...... */ 注释若干行
```

### 2. C

C中的注释通常采样形如 `/* …………*/` 的样式，后来因为C++中提供了以 `//` 作为开始的单行注释，而C++是兼容C的，所以二者已不再严格区分，在支持C++的编译器中写C程序，两种形式都可使用。

### 3. C++

C++ 中单行注释以 `//` 作为开始；

多行注释采用形如 `/* …………*/` 的样式。


### 4. Python


Python中单行注释以井号 `# `开始；

多行注释以三个单引号 triple-quotes `'''` 作为开始，再以同样的`'''`作为结束。

### 5. JavaScript

单行注释以 `//` 开头。多行注释以 `/*` 开始，以 `*/` 结尾。

### 6. Visual Basic

以Rem关键字开头并且Rem关键字与注释内容之间要加一个空格。注释可以是单独的一行也可以写在其他语句行的后面。如果在其他语句行后使用Rem关键字则必须使用冒号`:`与语句隔开。

或者，使用单引号 `'` 后面跟注释内容。使用一个撇号`'`来代替Rem关键字。若使用撇号则在其他语句行使用时不必加冒号。


### 7. R

R 中单行注释以井号 `#` 开始；R 中没有类似C++中 `/*....*/`这样的支持整块注释和嵌套注释的语法。

### 8. Matlab

在 MATLAB中, 百分号 `%` 可以引领一个单行注释；

多行注释可以用 `%{` 作为开始，并以 `%}` 作为结束，而且允许嵌套。

### 9. Haskell

Haskell 中单行注释 以 `--` (two hyphens) 开始；

多行注释则以  `{-` 开始，并以 `-}` 结尾。

<img src="https://fzuo.github.io/assets/img/program/program01.png" width="350">

Hasskell[官网](https://www.haskell.org/)


### 10. Prolog


SWI-Prolog 中支持形如 `/* ... */` 的嵌套注释。而 ISO 标准则允许形如 `/* ... /* ... */` 的注释, SWI-Prolog 会搜索注释的终止符 `*/`。

<img src="https://fzuo.github.io/assets/img/program/program02.png" width="520">

`关于Prolog的一些资源`

* 两个推荐使用的IDE

+ Visual Prolog (http://www.visual-prolog.com/) 商业版，功能强大，可视化

+ SWI_Prolog (http://www.swi-prolog.org/) 免费版，灵巧方便，值得推荐

* 一个在线教程——[Learn Prolog Now!](http://www.learnprolognow.org/)

<span style="color:blue">**（本文完）**</span>
