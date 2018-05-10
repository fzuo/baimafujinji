---
layout: post
title: Linux中编写并使用Shared Library
category: 工具
tags: Jekyll
description: 在这个博客里，我使用了bootstrap作为前端框架，另外通过比较选择了一个还算满意的代码高亮方式
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

```
gcc -Wall -g -fPIC -shared -o libmax.so max.c
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
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib04.png" width="550">
</p>

当然，我们前面强调过LD_LIBRARY_PATH的值只会被临时的修改，一旦你重启一个新的命令行窗口，Linux仍然会不认得libmax.so。所以在前面，你仍然要按下面这样的方法重新设定一下LD_LIBRARY_PATH的值。

<p align="center">
<img src="https://fzuo.github.io/assets/img/shared_lib/shared_lib05.png" width="450">
</p>

-----------------------------
### 参考文献：

【1】http://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html （最后浏览日期2017年10月）<br>
【2】https://www.cnblogs.com/jiqingwu/p/linux_dynamic_lib_create.html （最后浏览日期2017年10月）
