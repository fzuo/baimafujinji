

> GLPK（GNU Linear Programming Kit:GNU线性编程工具）是用于建立线性规划LP和混合型整数规划MIP问题的建模语言工具包，同时可以用来对模型进行最优化求解。网上已经提供了较多的在Windows下基于Microsoft Visual Studio配置GLPK的方法，这里我们来谈谈在Mac电脑中基于Xcode配置GLPK的方法。

首先到GLPK的官网上（http://www.gnu.org/software/glpk/）下载安装包，如下：

进入到Downloading页面，你会发现版本列表（如下所示），下载最新版本的tar压缩包，撰写本文时最新的GLPK版本号是4.60。

假设压缩包被下载到本机的Downloads文件夹，那么接下来打开terminal，并进入到该目录下：



解压对应的压缩包：



然后调用configure程序来执行配置（配置过程会需要几分钟，请耐心等待）：

接下来make程序，如下：

然后用管理员权限执行安装命令，引用用到了sudo，所以会被要求输入密码：

现在 you should have a working GLPK library. 可以来check一下：

我们已经完成了安装，但是这个工具箱还是不能直接用。是的，我们还需要在Xcode中进行配置。先是配置hearder path。如下图所示在搜索框里输入关键词，然后在对应的条目中Add the path "/usr/local/include"。


同理配置library search paths，如下图所示，Add the path "/usr/local/lib"


最后配置Linker flag，如下图所示，Add the additional linker flag: "-lglpk"



到此为止，所有的配置工作就都完成了。此后在你的project中的源文件里使用#include "glpk.h"便可轻松地应用GLPK中提供的各种功能了！GLPK的documents中提供了一些例子也非常便于读者自学入门，可以一并参考。

