---
layout: post
title: 哈代-温伯格（Hardy-Weinberg）定律
category: 围炉夜话
tags: 奇妙的数学
keywords: Hardy,Weinberg
---


哈代-温伯格（Hardy-Weinberg）定律是遗传学中一个著名的结论，而且它也是一个非常有名的应用数学问题。<img align="right" src="https://fzuo.github.io/assets/img/vs_msb802/GHHardy.png" width="180">

众所周知，自孟德尔的豌豆实验以来，遗传学逐渐成为生物学研究中非常重要的领域。根据基本的高中生物学常识我们知道，人眼的颜色受两个基因的控制：G表示棕色（brown），g表示蓝色（blue），G对g呈显性。所以，如果一个人眼睛颜色的基因为GG或Gg，则他的眼睛颜色都表现为brown，只有当其眼睛颜色的基因为gg时，他眼睛的颜色才表现为blue。于是人们就疑问：如果按照这个规律逐渐繁衍下去，那么蓝色眼睛的人会不会越来越少直至消失？这个争议在二十世纪初的生物学界引起了极大的讨论和研究兴趣。最终，英国大数学家哈代（也就是电影《知无涯者》中拉马努金的老师）和德国医生温伯格几乎同时从数学的角度给出了这个问题的证明（约1908年）。而且，据说哈代是在跟生物学家打板球的时候无意间听说到这个有趣的生物学问题的。

哈代-温伯格定律表明：对于一个大且随机交配的种群,基因频率和基因型频率在没有迁移、突变和选择的条件下会保持不变。本文将从矩阵（随机过程/马尔科夫链）的角度来阐释这个问题。而其中蕴含的思想早已超出生物学的范畴，在多个方面得到应用，甚至包括机器学习中的LDA模型。

分三种情况来讨论：假设父亲的基因是GG，则子代基因的概率分布入下图A矩阵所示；假设父亲的基因是Gg，则子代基因的概率分布入下图B矩阵所示；假设父亲的基因是gg，则子代基因的概率分布入下图C矩阵所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/hardy_w_01.png" width="580">
</p>

现在假设当前这一代中GG、Gg、gg分布的几率向量为$$W_0=(p, q, r)^T$$。那么就可以写出转移矩阵为：$$M_0=pA+qB+rC$$。此处一个可能令人困惑的地方在于，转移矩阵跟当前这一代中的基因分布有关。如果下一代中的$$W_1=(p', q', r')^T$$发生变化，那么似乎转移矩阵也会不一样。这样就无法满足马尔科夫过程的假设。但事实上可以证明：$$M_0=M_1=M_2=\cdots$$

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/hardy_w_02.png" width="520">
</p>

于是，下一代人口中各种基因携带者的几率分布向量（经过化简）为：

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/hardy_w_03.png" width="210">
</p>

同理，$$M_1=p'A+q'B+r'C$$，于是有

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/hardy_w_04.png" width="320">
</p>

也就得到$$M_0=M_1=M_2=\cdots$$，所以这确实是一个马尔科夫链问题。如果$$W_0$$表示这一代的基因分布，$$MW_0$$表示下一代的基因分布，$$M^2W_0$$表示再一代的基因分布，则有$$M(MW_0) = MW_0$$。In conclusion, the games reach equilibrium after the first generation. 这也就是Hardy-Weinberg Law的主要结论。


<span style="color:blue">**（本文完）**</span>

<script> MathJax.Hub.Queue(["Typeset",MathJax.Hub]); </script> 
