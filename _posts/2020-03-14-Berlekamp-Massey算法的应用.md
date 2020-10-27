---
layout:     post                    # 使用的布局（不需要改）
title:       BM算法                   # 标题 
subtitle:                           #副标题
date:       2020-03-14              # 时间
author:     ONLYUNIVERSE            # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
mathjax: true
tags:                               #标签
    Cryptography
---

### Berlekamp-Massey算法

BM算法是用来求解一个数列的最短线性递推式的算法
在本文中，BM算法具体被用于求解产生已知序列的线性移位寄存器的特征多项式及级数

### 一些定义

- R<sub>0</sub>={}  &emsp;&emsp;&emsp;&emsp;&emsp;//初始递推式

- cnt &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//递推式的修改次数

- delta<sub>i</sub>=a<sub>i</sub> - $\sum_{j=1}^m$ r<sub>j</sub>*a<sub>i-j</sub>&emsp;&emsp;&emsp;//递推式的正确性的判断，其中r<sub>i</sub>是递推式的元素，m是递推式中元素的个数

- fail<sub>k</sub>=i&emsp;&emsp;&emsp;&emsp;&emsp; //表示R第k次出错，在a<sub>i</sub>处出错

- mul=$delta_i \over delt_{fail_{cnt-1}}$

- R′={ &emsp;&emsp;(i-fail<sub>cnt-1</sub>-1个0)&emsp;&emsp;, &emsp;mul,&emsp;-mul·R<sub>cnt-1</sub>}

- R<sub>cnt+1</sub>=R<sub>cnt</sub>+R′&emsp;&emsp;//(按位相加)

### 计算步骤

$$ 一、delta_i = \begin{cases}
delta_i=0 & 不修改 \\
delta_i\neq0 & 计算fail_{cnt}
\end{cases}$$

$$ 二、faila_i=i\begin{cases}
cnt=0 & R_{cnt}=\{\overbrace{0,0,···,0}^{i个}\} \\
cnt\neq0 & 转三
\end{cases}$$

$$ 三、cnt\neq0 \begin{cases}
mul={delta_i \over delt_{fail_{cnt-1}}\ } & \\
R′=\{\overbrace{0,0,···,0}^{i-fail_{cnt-1}个},mul,-mul·R_{cnt-1}\}&\\
R_{cnt+1}=R_{cnt}+R′(按位相加)
\end{cases}$$

### 实例

令待测序列S=10101111

- R<sub>0</sub>={} cnt=0

- a<sub>1</sub>=1 &emsp;delt<sub>1</sub>=1&emsp;&emsp;fail<sub>0</sub>=1&emsp;&emsp;R<sub>1</sub>={0} &emsp;cnt=1

- a<sub>2</sub>=0 &emsp;delt<sub>2</sub>=0 &emsp;&emsp;不修改

- a<sub>3</sub>=1 &emsp;delt<sub>3</sub>=1&emsp;&emsp;fail<sub>1</sub>=3&emsp;&emsp;mul=1&emsp; R′={0,1,0} &emsp;R<sub>2</sub>={0,1,0} &emsp;cnt=2

- a<sub>4</sub>=0 &emsp;delt<sub>4</sub>=0 &emsp;&emsp;不修改

- a<sub>5</sub>=1 &emsp;delt<sub>5</sub>=0 &emsp;&emsp;不修改

- a<sub>6</sub>=1 &emsp;delt<sub>6</sub>=1  &emsp;&emsp;fail<sub>2</sub>=6&emsp;&emsp;mul=1 &emsp;&emsp;R′={0,0,1,0}&emsp;R<sub>3</sub>={0,1,1,0}&emsp;&emsp;cnt=3

- a<sub>7</sub>=1 &emsp;delt<sub>7</sub>=0 &emsp;&emsp;不修改

- a<sub>8</sub>=1 &emsp;delt<sub>8</sub>=-1 &emsp;fail2=8 &emsp;mul=-1 R′={0,-1,0,1,0}&emsp;R<sub>4</sub>={0,0,1,1,0}&emsp;&emsp;cnt=4

>综上，由R<sub>4</sub>={0,0,1,1,0}&emsp;&emsp;cnt=4可知目前为止得到的特征多项式为

1+0·x+0·x<sup>2</sup>+1·x<sup>3</sup>+1·x<sup>4</sup>+0·x<sup>5</sup>

级数为4.