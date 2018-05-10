---
layout: post
title: Linux中编写并使用Shared Library
category: 技巧与工具
tags: Linux
keywords: 动态链接库,shared library
---

在Linux中，shared libraries are libraries that are loaded by programs when they start. When a shared library is installed properly, all programs that start afterwards automatically use the new shared library。 

可见，Linux中的shared libraries 文件为共享库，与windows下dll类似，是动态加载的。在链接动态库生成可执行文件时，并不会把动态库的代码复制到执行文件中，而是在执行文件中记录对动态库的引用。程序执行时，再去加载动态库文件。如果动态库已经加载，则不必重复加载，从而能节省内存空间。

Every shared library has a special name called the "soname". The soname has the prefix "lib", the name of the library, and ended with the phrase ".so"。例如，libmax.so。

下面通过一个例子来简单演示创建并使用.so文件的方法。首先，建立一个名为 max.c 的源文件。

```c
int max(int a, int b)  
{  
    int result = a > b? a:b;  
    return result;  
} 
```

接下来在命令行窗口中编译生成共享库：

```
gcc -Wall -g -fPIC -shared -o libmax.so max.c
```

其中， -Wall -g 是供调试使用的，并非是必须的，所以你也可以写成

```
gcc -fPIC -shared -o libmax.so max.c 
```

其中， -fPIC是编译选项，PIC是 Position Independent Code 的缩写，表示要生成位置无关的代码，这是动态库需要的特性； -shared是链接选项，告诉gcc生成动态库而不是可执行文件。

此外，上面的命令实际上可以分为编译和链接两步，所以你写成下面这样的形式也是可以的：

```
gcc -fPIC -c max.c -o max.o   
gcc -shared max.o -o libmax.so   
```

接下来，为了让用户知道我们的动态库中有哪些接口可用，我们需要编写对应的头文件。建立 max.h ，输入以下代码：

```c
#ifndef __MAX_H__  
#define __MAX_H__  
  
int max(int a, int b);  
  
#endif  
```

最后，为了测试上面生成的.so文件，建立一个test.c文件：

```c
#include <stdio.h>  
#include "max.h"  
  
int main(int argc, char *argv[])  
{  
    printf("call max function results is %d.\n", max(2256,3375));  
    return 0;  
}  
```

现在，你可以在命令行中使用 gcc test.c -L. -lmax 来生成一个a.out文件，其中-lmax表示要链接libmax.so。-L.表示搜索要链接的库文件时包含当前路径。注意，如果同一目录下同时存在同名的动态库和静态库，比如 libmax.so 和 libmax.a 都在当前路径下，则gcc会优先链接动态库。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib01.png" width="550">
</p>

但是当我们要执行a.out文件时，如上图所示，Linux报出了下面这个错误信息：

```
./a.out: error while loading shared libraries: libmax.so: cannot open shared object file: No such file or directory
```

这主要是因为Linux找不到所需动态加载的libmax.so文件。为了解决这个问题你可以使用下面这行指令：

```
LD_LIBRARY_PATH=. ./a.out  
```

其中的LD_LIBRARY_PATH=. 是告诉 a.out，先在当前路径寻找链接的动态库。如下图所示，此时程序便能正常运行了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib02.png" width="450">
</p>

或者，你也可以在命令行中使用下面的命令，临时地调整LD_LIBRARY_PATH的值，令其包含你存放libmax.so文件的路径：

```
$ export LD_LIBRARY_PATH=/home/airobot/Desktop/shared_lib:$LD_LIBRARY_PATH
```

如下图所示，此时程序可以正常执行了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib03.png" width="550">
</p>

但是，如果你的主程序是C++，例如向下面这样的一个文件mymain.cpp：

```c
#include<iostream>  
#include"max.h"  
  
using namespace std;  
  
int main(void)  
{  
    cout<<"call max function results is: "<<max(2256,3375)<<endl;  
    return 0;  
}  
```

那么假如你仍然按照之前的方法来做是行不通的。因为libmax这个库仅适合C使用，C++并不适合，如果想编译一个可以供C++使用。那么头文件（max.h）就需要改变，例如，修改max.h文件如下：

```c
#ifndef MAX_H_  
#define MAX_H_  
  
#ifdef __cplusplus  
extern "C"  
{  
#endif  
int max(int a, int b);  
#ifdef __cplusplus  
}  
#endif  
  
#endif  
```

其中用到了C++的一个宏：__cplusplus   当用g++编译的时候，就可以识别这个宏。此时，执行文件mymain便可以输出预期的结果，如下图所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib04.png" width="550">
</p>

当然，我们前面强调过LD_LIBRARY_PATH的值只会被临时的修改，一旦你重启一个新的命令行窗口，Linux仍然会不认得libmax.so。所以在前面，你仍然要按下面这样的方法重新设定一下LD_LIBRARY_PATH的值。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib05.png" width="450">
</p>

-----------------------------
### 参考文献：

【1】[文章链接1](http://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html) （最后浏览日期2017年10月）<br>
【2】[文章链接2](https://www.cnblogs.com/jiqingwu/p/linux_dynamic_lib_create.html) （最后浏览日期2017年10月）
