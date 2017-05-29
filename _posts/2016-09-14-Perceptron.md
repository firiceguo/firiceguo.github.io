---
layout: post
title: "Perceptron"
date: 2016-09-14 12:39
tags: MachineLearning
categories: MachineLearning
thumbnail:  book
comments: true
description: Questions about Perceptron
---

## About Perceptron

### 感知机用在哪些地方？

二维平面中线性可分的二分类问题。

> 线性不可分的问题首先是由 Marvin Minsky 提出的 XOR 问题，用SVM解。

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

### 如何确定算法收敛？

假定w和x都是单位向量，即长度都为1，则有一个距离决策边界的最近距离：

$$ \gamma = \min_i |(w^*)^T x_i| $$

犯错误次数`M`：

$$ M \le (\frac 1 \gamma)^2 $$

证明如下（摘自《统计学习方法》P31）：

![img]({{ site.baseurl | prepend:site.url}}/images/perceptron-prove.png){: .center-image }

------
Update 2016/12/22
------

## 神经网络 (Neural Network) -- 激活函数们

感知机就是早期的单层(前向)人工神经网络，因此无法处理线性不可分问题(回路)，由此的两种解决方案，一种是SVM，一种就是基于反向传播算法的多层神经网络。

神经元的基本模型(基于Sigmoid激活函数)是这样的：

![img]({{ site.baseurl | prepend:site.url}}/images/Neural.png){: .center-image }

### 公式

From [here](https://www.quora.com/What-is-special-about-rectifier-neural-units-used-in-NN-learning)

1. `Sigmoid` 激活函数：

	$$ f(x) = \frac 1 {1+e^{-x}} $$

2. `Tanh` 激活函数：

	$$ f(x) = tanh(x) $$

3. `Rectified linear unit (ReLU)` 激活函数：

	$$ f(x) = \sum ^{inf}_{i=1}\sigma (x-i+0.5) \approx log(1+e^x) $$

	其中：
	
	$$ \sum ^{inf}_{i=1}\sigma (x-i+0.5) $$ 被称作 `stepped sigmoid`
	
	$$ log(1+e^x) $$ 被称作 `softplus function`

`softplus function` 近似为 `max function(or hard function)`，`max function` 就是 `Rectified Linear Function (ReL)`

`max function` 的一个例子: $$ max(0, x+N(0, 1)) $$

### 对比

下图是三个函数的对比图：

![img]({{ site.baseurl | prepend:site.url}}/images/ReL-compare.png){: .center-image }

`Sigmoid function` 和 `ReL function` 的主要不同点：

- `Sigmoid function` 的范围是 [0, 1] 而 `ReL function` 的范围是 [0, inf]。

- 在 x 增加时，`Sigmoid function` 会出现梯度消失的情况，而 `ReL function` 不会。


### 参考链接：

- 一篇论文：[Deep Sparse Rectifier Neural Networks](www.jmlr.org/proceedings/papers/v15/glorot11a/glorot11a.pdf)

- 主要讲ReLU：[ReLu(Rectified Linear Units)激活函数](http://www.cnblogs.com/neopenx/p/4453161.html)

- 上面公式和对比的来源：[What is special about rectifier neural units used in NN learning?](https://www.quora.com/What-is-special-about-rectifier-neural-units-used-in-NN-learning)

- 激活函数的一个概括：[神经网络之激活函数(Activation Function)](http://blog.csdn.net/cyh_24/article/details/50593400)
