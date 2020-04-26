---
layout:     post                    # 使用的布局（不需要改）
title:       BM算法                   # 标题 
subtitle:                           #副标题
date:       2020-03-14              # 时间
author:     ONLYUNIVERSE            # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    Cryptography
---

### Berlekamp-Massey算法

BM算法是用来求解一个数列的最短线性递推式的算法
在本文中，BM算法具体被用于求解产生已知序列的线性移位寄存器的特征多项式及级数

### 一些定义

- R~0~={}  &emsp;&emsp;&emsp;&emsp;&emsp;//初始递推式

- cnt &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//递推式的修改次数

- delta~i~=a~i~ - $\sum_{j=1}^m$ r~j~*a~i-j~&emsp;&emsp;&emsp;//递推式的正确性的判断，其中r~i~是递推式的元素，m是递推式中元素的个数

- fail~k~=i&emsp;&emsp;&emsp;&emsp;&emsp; //表示R第k次出错，在a~i~处出错

- mul=$delta~i~ \over delt~fail~cnt-1~~$

- R′={ &emsp;&emsp;(i-fail~cnt-1~-1个0)&emsp;&emsp;, &emsp;mul,&emsp;-mul·R~cnt-1~}

- R~cnt+1~=R~cnt~+R′&emsp;&emsp;//(按位相加)

### 计算步骤

$$ 一、delta~i~=\begin{cases}
delta~i~=0 & 不修改 \\
delta~i~\neq0 & 计算fail~cnt~
\end{cases}$$

$$ 二、faila~i~=i\begin{cases}
cnt=0 & R~cnt~=\{\overbrace{0,0,···,0}^{i个}\} \\
cnt\neq0 & 转三
\end{cases}$$

$$ 三、cnt\neq0 \begin{cases}
mul={delta~i~ \over delt~fail~cnt-1~~\ } & \\
R′=\{\overbrace{0,0,···,0}^{i-fail~cnt-1~个},mul,-mul·R~cnt-1~\}&\\
R~cnt+1~=R~cnt~+R′(按位相加)
\end{cases}$$

### 实例

令待测序列S=10101111

- R~0~={} cnt=0

- a~1~=1 &emsp;delt~1~=1&emsp;&emsp;fail~0~=1&emsp;&emsp;R~1~={0} &emsp;cnt=1

- a~2~=0 &emsp;delt~2~=0 &emsp;&emsp;不修改

- a~3~=1 &emsp;delt~3~=1&emsp;&emsp;fail~1~=3&emsp;&emsp;mul=1&emsp; R′={0,1,0} &emsp;R~2~={0,1,0} &emsp;cnt=2

- a~4~=0 &emsp;delt~4~=0 &emsp;&emsp;不修改

- a~5~=1 &emsp;delt~5~=0 &emsp;&emsp;不修改

- a~6~=1 &emsp;delt~6~=1  &emsp;&emsp;fail~2~=6&emsp;&emsp;mul=1 &emsp;&emsp;R′={0,0,1,0}&emsp;R~3~={0,1,1,0}&emsp;&emsp;cnt=3

- a~7~=1 &emsp;delt~7~=0 &emsp;&emsp;不修改

- a~8~=1 &emsp;delt~8~=-1 &emsp;fail2=8 &emsp;mul=-1 R′={0,-1,0,1,0}&emsp;R~4~={0,0,1,1,0}&emsp;&emsp;cnt=4

>综上，由R~4~={0,0,1,1,0}&emsp;&emsp;cnt=4可知目前为止得到的特征多项式为
>>1+0·$x$+0·$x^2_ {}$+1·$x^3_ {}$+1·$x^4_ {}$+0·$x^5_ {}$
级数为4
