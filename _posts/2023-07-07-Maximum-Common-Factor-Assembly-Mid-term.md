---
layout:     post
title:      Maximum Common Factor
subtitle:   Assembly Mid-term Question 14
date:       2023-07-07
author:     KO
header-img: img/galaxy-min.jpg
catalog: true
tags:
    - Algorism
---


### 原题 
Write an x86 assembly program that finds the maximum common factor of two positive integers stored in the EAX and EBX registers and stores the result in ECX. You are only allowed to use only one label. You are allowed to write a maximum 25 lines of code. Violating any of these conditions will result in losing 4 points.

### 有关寻找最大公约数
求两数的最大公约数，一共有四种方法：暴力穷举法、更相减损法、辗转相除法、stein 算法。这边只讲两种。

## 暴力穷举法
正如名字所说，暴击穷举法就是简单粗暴地把 1~ y（前面已经假设 x > y）都列出来分别判断是否为 x、y 的约数，然后再找到其中最大的一个。
> a/b, b/b, 如果两个余数都是0，就ok, 否则a/(b-1) b/(b-1)

## 辗转相除法
辗转相除法是由古希腊数学家欧几里得在其著作《The Elements》中提出的，所以辗转相除法又名欧几里得算法。

步骤
假设要用辗转相除法求 36 和 24 的最大公约数，则要经历以下步骤：

36 ÷ 24 = 1 …… 12
24 ÷ 12 = 2 …… 0
12 为 36 和 24 的最大公约数

即先用 x 除以 y
若余数为 0 则 y 为两数的最大公约数；若余数不为零，则令 x = y，y = 余数，重复步骤 1 直到余数为 0，此时的 y 为两数的最大公约数。

### 用辗转相除法做14题
## 两个Label的
## 一个Label的

