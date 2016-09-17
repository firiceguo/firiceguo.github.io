---
layout: post
title: "Perceptron"
date: 2016-09-14 12:39
tags: MachineLearning
categories: MachineLearning
thumbnail:  book
description: Questions about Perceptron
---

## About Perceptron

### 感知机用在哪些地方？

二维平面中线性可分的二分类问题。

> 线性不可分的问题首先是由 Marvin Minsky 提出的 XOR 问题，用SVM解。
{: .note}

### 感知机是怎么工作的？

对于分布`D`中独立同分布的数据`{ (xi, yi) : 1 <= i <= n}`,

$$ y = sign(f_w(x)) = sign(w^T x) $$

其中`y = sign(z)`是符号函数，当`z > 0`时`y = +1`,当`z < 0`时`y = -1`

算法过程如下：

1. 将权值初始化为0，即`w1=0`，令`t=1`；

2. 选取一个初始数据点`x`，使`w1·x > 0`，并将其设为`+1`；

3. 搜索错误的数据点：
	
	- 如果是`+1`却将其分为了`-1`：`w(t+1) := w(t) + x`
	- 如果是`-1`却将其分为了`+1`：`w(t+1) := w(t) - x`

4. `t = t + 1`，并且重复第三步

> 这里的 w 对应分类线的法向量
{: .note}

### 如何确定算法收敛？

假定w和x都是单位向量，即长度都为1，则有一个距离决策边界的最近距离：

$$ \gamma = \min_i |(w^*)^T x_i| $$

犯错误次数`M`：

$$ M \le (\frac 1 \gamma)^2 $$

证明如下（摘自《统计学习方法》P31）：

![img]({{ site.baseurl | prepend:site.url}}/images/perceptron-prove.png){: .center-image }

