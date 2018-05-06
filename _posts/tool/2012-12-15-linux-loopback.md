---
layout: post
title: 在Linux中创建回环设备的方法
category: 技巧与工具
tags: Linux
keywords: Linux,Loopback,回环设备
---

> 你有没有想过在Linux的文件管理系统中再创建一个文件系统，就像在Windows中创建一个新的磁盘分区那样（但其实并不必非得直接那么做）。这时你就需要用到回环设备（loop device）。

在Linux中，回环设备允许用户以一个普通磁盘文件虚拟一个块设备。设想一个磁盘设备，对它的所有读写操作都将被重定向到读写一个名为 virtualfs 的普通文件而非操作实际磁盘或分区的轨道和扇区。

当然，virtualfs这个名字你可以任意取，但这个文件必须存在于一个实际的磁盘上，而这个实际的磁盘必须比虚拟的磁盘（virtualfs）容量更大。回环设备允许你这样使用一个普通文件。With a loopback device in Linux, one can easily create a Linux loopback file system on a regular disk file, and not directly using a disk partition!

回环设备的使用与其它任何块设备相同。特别是，你可以在这个设备上创建文件系统并像普通的磁盘一样将它挂载在系统中。这样的一个将全部内容保存在一个普通文件中的文件系统，被称为虚拟文件系统（virtual file system）。

下面就来演示用一个regular磁盘文件创建一个Linux回环文件系统的步骤。

`【Step1】` 创建一个用于承载虚拟文件系统的空文件。这个文件的大小将成为挂载后文件系统的大小。创建指定大小文件的简单方法是通过 dd 命令。这个命令以块为单位（通常为 512 字节，或者你也可以自定义块大小）从一个文件向另一个文件复制数据。/dev/zero 文件则是一个很好的数据来源。

要在根目录下（root directory）建立一个 30 MB 大小（zero-filled）的名为 virtualfs 的文件可以通过以下命令：

```
$sudo dd if=/dev/zero of=/virtualfs bs=1024 count=30720  
```

你可以使用下面的命令来查看刚创建之文件的情况：

```
$ls -l /virtualfs
```

`【Step2】` 回环设备以 /dev/loop0、/dev/loop1 等命名。每个设备可虚拟一个块设备。为了确认当前系统是否有在使用回环设备，你需要使用下面的语

```
$losetup /dev/loop0
```

如果 /dev/loop0 已经存在，你会得到类似下面的结果。然后你就需要把 /dev/loop0 替换成 /dev/loop1, 或者再把 /dev/loop1替换成/dev/loop2, 并以此类推，直到找到一个空的回环设备为止。

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback01.png" width="500">
</p>

现在假设 /dev/loop0 并没有被占用，如下图所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback02.png" width="500">
</p>

`【Step3】` 接下来使用losetup命令来把常规文件或块设备（/dev/loop0）关联到一个loop文件（virtualfs）上。注意只有超级用户才有权限设置回环设备。

```
$sudo losetup /dev/loop0 /virtualfs  
```

到这一步，你可以在命令行中输入echo $? 来确认 the previous step is completed successfully without error。在进行源代码编译，或者执行命令无法确认所执行的命令是否成功执行的情况下，我们都会使用 echo $? 来进行测试。如果返回值是0，就是执行成功；如果是返回值是0以外的值，就是失败。

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback03.png" width="500">
</p>

这里面其实利用了“进程生命周期”的原理：当一个进程执行完毕时，该进程会调用一个名为 \_exit 的例程来通知内核它已经做好“消亡”的准备了。该进程会提供一个退出码（一个整数）表明它准备退出的原因。按照惯例，0用来表示正常的或者说“成功”的终止。也就是说我们在执行 echo $? 时反回的值就是进程的退出码。而且，这个退出码是由刚刚执行完的进程提供给系统内核的。

`【Step4】` 接下来需要在回环设备上创建一个 Linux EXT3 文件系统（with 1% reserved block count）,而该文件当前已经被关联到一个普通的磁盘文件上了。为此输入：

```
$sudo mkfs -t ext3 -m 1 -v /dev/loop0
```

如下图所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback04.png" width="500">
</p>

这样一来，我们就在已有的文件系统中又创建了一个文件系统，而这个过程并非采用对磁盘进行直接分区的方式。

`【Step5】` 然后我们在需要在 /mnt 中创建一个文件夹来作为挂载点（mount point），于是输入

```
$sudo mkdir /mnt/vfs
```

接下来就是把回环文件系统（其实就是一个普通的磁盘文件）挂载（mount）到上面刚刚创建的目录上（/mnt/vfs），这样就算完成了一个“regular” Linux EXT3文件系统的创建。为此，需要输入

```
$sudo mount -t ext3 /dev/loop0 /mnt/vfs  
```

如下图所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback05.png" width="500">
</p>

所谓的『挂载』就是利用一个目录当成进入点，将磁盘分区槽的数据放置在该目录下；也就是说，进入该目录就可以读取该分割槽（partition）的意思。这个动作我们称为『挂载』，那个进入点的目录我们称为『挂载点』。由於整个Linux系统最重要的是根目录，因此根目录一定需要挂载到某个分割槽的。至於其他的目录则可依使用者自己的需求来给予挂载到不同的分割槽。

现在，所有的 Linux 文件系统相关的命令就都可以在上面创建的这个文件系统中使用了。例如，你可以输入 df -h来确认其 “disk usage”，或者输入 tune2fs -l /dev/loop0 来输出它的文件系统设置，等等。例如下面的例子中，我们在其中创建了一个文件夹tmp_dir，然后用ls来列出这个磁盘中的内容。请注意 lost+found 是一个由 mkfs 自动建立的文件夹一旦文件系统被破坏，部分数据被回复但没有与任何文件关联起来，将被放置在这个文件夹中。

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback05.png" width="500">
</p>

注意，输入sudo echo 'hello world!'>test.txt是不可以的，这会导致Permission denied。因为The redirection（>） is done by the shell before sudo is even started. 所以你要使用上面图中所示的指令。另外，使用nano来创建或编辑文件，也要使用超级用户权限，即sudo nano test.txt，否则就可能无法保存。

`【Step6】` 最后在结束使用后，需要卸载这个文件系统。事实上，你可以删除文件，或者之后再次挂载并使用其中的文件。你甚至可以将这个文件复制到远程主机并在那里挂载、使用——文件系统的内容完全不会受到影响。
前面我们提过，losetup被用来创建和控制loop文件，把常规文件或块设备关联到一个loop文件上。但是当使用 -d 时就表示删除关联到loop设备的文件文件或设备。所以我们首先输入

```
$sudo losetup -d /dev/loop0
```

然后再使用下面的命令

```
$sudo umount /mnt/vfs
```

便可以将已经挂载的回环文件系统卸载了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_loopback/linux_loopback06.png" width="500">
</p>

-----------------------------
### 参考

【1】http://www.walkernews.net/2007/07/01/create-linux-loopback-file-system-on-disk-file/ <br>
【2】鸟哥的Linux私房菜，人民邮电出版社
