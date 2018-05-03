---
layout: post
title: 有用的Linux命令整理
category: 技巧与工具
tags: Linux
keywords: Linux,命令
---


### System

1. `man` command —— show the manual for command

    ```man whereis```

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_01.png" width="500">
</p>

2. `whereis`app —— show possible locations of app

    $ whereis gcc
    gcc: /usr/bin/gcc /usr/lib/gcc /usr/share/man/man1/gcc.1.gz

3. `which` app —— show which app will be run by default

    $ which gcc
    /usr/bin/gcc

4. `whoami` —— show your username (who you are logged in as)

    $ whoami
    airobot

### File/Directory Basics

5. pwd —— show the present working directory
$ pwd
/home/airobot/Desktop/dev

6. cd
cd dir —— change directory to dir
cd .. —— go up one directory
cd / —— go to the root directory
cd ~ —— go to your home directory
cd - —— go to the last directory you were just in

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_02.png" width="400">
</p>

### File  Viewing

7. cat —— output the contents of file
8. head —— output the first 10 lines of file
9. tail —— output the last 10 lines of file
10. nl —— number lines

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_03.png" width="400">
</p>

### File Properties/Permissions

11. file —— identify file types
12. stat —— display file attributes

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_04.png" width="400">
</p>

13. chmod ugo file —— change permissions of file to ugo, where u is the user's permission, g is the group's permissions, and o is the everyone else's permission. The walues of u, g, and o can bu any number between 0 and 7. (4: read, 2: write, 1: execute)
e.g. 
chmod 777: read, write, execute for all （参考上图中给出的例子）
chmod 755: rwx for owner, everyone else can rx
chmod 600: you can read and write
chmod 644: you can read and wirte, and everyone else can only read
14. chgrp —— change file group
15. chown —— change file owner

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_05.png" width="400">
</p>

### IO Redirection

16. cat > file —— places standard input to file
例如，在下图中，我们首先在命令上输入 cat > my_file，然后系统会自动在当前文件夹下创建一个名为my_file的文件，然后我们在Terminal上输入的文字将被写入my_file中，直到我们在键盘上输入CTRL + d 时返回。这时我们打开my_file文件，可以发现之前的内容已经被成功写入了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_06.png" width="400">
</p>

17. cmd > file —— Standard output (stdout) of cmd to file

<p align="center">
<img src="https://fzuo.github.io/assets/img/linux_command/linux_command_07.png" width="400">
</p>

18. cmd >> file ——  Append stdout to file


Process Management
19. ps —— show snapshot of processes
ps命令用于报告当前系统的进程状态。可以搭配kill指令随时中断、删除不必要的程序。ps命令是最基本同时也是非常强大的进程查看命令，使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等等，总之大部分信息都是可以通过执行该命令得到的。
20. top —— show real time processes
 top命令可以实时动态地查看系统的整体运行情况，是一个综合了多方信息监测系统性能和运行信息的实用工具。通过top命令所提供的互动式界面，用热键可以管理。 

21. kill pid —— kill process with id pid
22. pkill name —— kill process with name name
23. killall name —— kill all processes with names beginning name

Searching
24. locate file —— find all instances of file using indexed database built from the updatedb command. Much faster than find
25. grep pattern files —— search for pattern in files
grep -i : case insensitive search
grep -o : only show matched part of file 

26. grep -r pattern dir —— search recursively for pattern in dir
27. grep -rn pattern dir —— search recursively for pattern in dir and show the line number found
28. command | grep pattern —— search for pattern in the output of command

Pipes and Command Lists

29. cmd1 | cmd2 —— stdout of cmd1 to cmd2
30. cmd1 |& cmd2 —— stderr of cmd1 to cmd2
31. cmd1 ; cmd2 —— run cmd1 then cmd2
32. cmd1 && cmd2 —— run cmd2 if cmd1 is successful
33. cmd1 || cmd2 —— run cmd2 if cmd1 is unsuccessfull

Shortcuts
CTRL+c —— stop current command
CTRL+w —— erases one word in the current line
CTRL+u —— erases the whole line
CTRL+a —— move cursor to the beginning of line
CTRL+e —— move cursor to the end of line
CTRL+b —— move cursor backward one character
CTRL+f ——  move cursor forward one character
ALT+b —— move cursor backward one word
ALT+f —— move cursor forward one word 

参考
http://gnulinux.guru/bash_cheatsheet.pdf
http://man.linuxde.net/

### 连续不中断执行

用`;`可以让多个命令连续知行，中间出现错误并不会中断后面命令，如

    mkdir test; mkdir test; rmdir test;

虽然第二条指令会报错，但是不会影响后面的指令，最后test目录不存在

### 出错停止后面指令

用`&&`分割的命令，如果没有错误会一直执行下去，出现错误立即中止，如

    mkdir test && mkdir test && rmdir test

这回在第二个指令处就中止了

### 一次正确即停止

用`||`分割的命令，如果有错误就一直执行下去，直到一次正确立即中止，如

    mkdir test || mkdir test || rmdir test
    mkdir test || mkdir test || rmdir test || mkdir test

第一次执行第一条指令就正确，后面的不执行

第二次执行前两条都错误，直到最后一条才正确，最后一条不再执行
