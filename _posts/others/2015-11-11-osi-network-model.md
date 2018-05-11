---
layout: post
title: 如何记住OSI七层协议模型
category: 闲话二三事
tags: Network
keywords: OSI网络模型
---


学过计算机网络的各位，一定知道OSI七层协议模型。OSI七层协议模型（或简称OSI七层模型），是一个定义得非常好的协议规范，被认为是现代开放性通信系统互连互通的标准参考模型。OSI模型有7层结构，每层都可以有几个子层。 OSI的7层从上到下分别是：

### <span style="color:red">7. 应用层——Application(Layer 7)</span>

与其它计算机进行通讯的一个应用，它是对应应用程序的通信服务的。例如，一个没有通信功能的字处理程序就不能执行通信的代码，从事字处理工作的程序员也不关心OSI的第7层。但是，如果添加了一个传输文件的选项，那么字处理器的程序员就需要实现OSI的第7层。

### <span style="color:red">6. 表示层——Presentation(Layer 6)</span>

这一层的主要功能是定义数据格式及加密。例如，FTP允许你选择以二进制或ASCII格式传输。如果选择二进制，那么发送方和接收方不改变文件的内容。如果选择ASCII格式，发送方将把文本从发送方的字符集转换成标准的ASCII后发送数据。在接收方将标准的ASCII转换成接收方计算机的字符集。

### <span style="color:red">5. 会话层——Session(Layer 5)</span>

它定义了如何开始、控制和结束一个会话，包括对多个双向消息的控制和管理，以便在只完成连续消息的一部分时可以通知应用，从而使表示层看到的数据是连续的，在某些情况下，如果表示层收到了所有的数据，则用数据代表表示层。

### <span style="color:red">4. 传输层—— Transport(Layer 4)</span>

这层的功能包括是否选择差错恢复协议还是无差错恢复协议，及在同一主机上对不同应用的数据流的输入进行复用，还包括对收到的顺序不对的数据包的重新排序功能。

### <span style="color:red">3. 网络层——Network(Layer 3)</span>

这层对端到端的包传输进行定义，它定义了能够标识所有结点的逻辑地址，还定义了路由实现的方式和学习的方式。为了适应最大传输单元长度小于包长度的传输介质，网络层还定义了如何将一个包分解成更小的包的分段方法。

### <span style="color:red">2. 数据链路层——Data Link(Layer 2)</span>

它定义了在单个链路上如何传输数据。这些协议与被讨论的各种介质有关。

### <span style="color:red">1. 物理层——Physical(Layer 1)</span>

物理层规范是有关传输介质的特性标准，这些规范通常也参考了其他组织制定的标准。连接头、帧、帧的使用、电流、编码及光调制等都属于各种物理层规范中的内容。物理层常用多个规范完成对所有细节的定义。


一个比较令人头疼的问题是如何不缺不漏而且按顺序记住这7层呢？脑动大开的网友给出了许多记忆口诀（不知道你喜欢哪个？）：

1. <span style="color:blue"><u>A</u>ll    <u>P</u>eople    <u>S</u>eem    <u>T</u>o    <u>N</u>eed    <u>D</u>ata    <u>P</u>rocessing</span>

2. <span style="color:blue"><u>P</u>lease   <u>D</u>o   <u>N</u>ot   <u>T</u>ell   <u>S</u>tupid   <u>P</u>eople   <u>A</u>nything</span>

3. <span style="color:blue"><u>P</u>lease   <u>D</u>o   <u>N</u>ot   <u>T</u>ake   <u>S</u>ales   <u>P</u>eople\'s   <u>A</u>dvice</span>

4. <span style="color:blue"><u>P</u>lease    <u>D</u>o   <u>N</u>ot   <u>T</u>hrow    <u>S</u>ausage   <u>P</u>izza   <u>A</u>way</span>

5. <span style="color:blue"><u>P</u>lease    <u>D</u>o   <u>N</u>ot   <u>T</u>each   <u>S</u>tudents  <u>P</u>ointless  <u>A</u>cronyms</span>

最后我们说明，七层协议中的高层，即7、6、5、4层定义了应用程序的功能，下面3层，即3、2、1层主要面向通过网络的端到端的数据流。每一层所包含的具体协议举例如下表：

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/osi7.png" width="450">
</p>

**（本文完）**
