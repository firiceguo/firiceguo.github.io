---
layout: post
title: "机器学习 —— 评估方法"
date: 2016-08-24 15:33
tags: MachineLearning
categories: MachineLearning
thumbnail:  books
comments: true
description: 一些设置训练集和测试集的方法
---

## 评估方法

包含m个样例的数据集: $$D = {(x_1, y_1), (x_2, y_2), ... , (x_m, y_m)}$$

训练集: T

测试集: S


## 留出法

直接将数据集分成训练集和测试集

常见的方法是对大约**2/3~4/5**的样本进行训练

>优点：简洁方便

>缺点：S过大，虽能包含大部分数据，但测试精度不高；S过小，虽测试精度提高，但和D相差过大，降低评估系统的“保真性”（fidelity）


## 交叉验证法（“k折交叉验证” k-fold cross validation）

将数据集分成k份，每一份都分为训练集和测试集两部分，将测试结果平均得到最终结果

k常取5，10，20

为了避免由于不同的划分产生的差别，通常随机使用不同的划分，并且重复p次，最终的结果是**p次k折交叉验证法**的均值


## 变形：留一法 (Leave-One-Out, LOO)

若D中有m个样本，当k=m时，每个子集包含一个样本

>优点：结果比较准确

>缺点：数据集太大时开销难以承受

>以上的方法都会引入一些因训练样本规模不同而导致的估计偏差


## 自助法 bootstrapping (有放回采样)

每次随机从D中挑出一个样本复制到D0中，重复m次，则在m次采样中始终不被采样到的概率取极限为：

$$ \lim_{m\to\infty}(1-\frac1m)^m \to \frac1e\approx 0.368 $$

即通过自助采样，初始数据集中有36.8%的数据没有出现在$$D_0$$中，我们可以将$$D_0$$作为训练集，$$D\setminus D_0$$作为测试集

> 优点：能减少训练规模不同所造成的影响，在数据集较小、难以有效划分训练/测试集时很有用，对于集成学习等方法有很大的好处

> 缺点：改变了初始数据集的分布，会引入估计偏差


-------
Update 2016/12/22
-------

## 验证集 & 测试集 (Validation set & Test set)

Pattern Recognition and Neural Networks, Ripley, B.D (1996):

- Training set: A set of examples used for learning, which is to fit the parameters [i.e., weights] of the classifier.

- Validation set: A set of examples used to tune the parameters [i.e., architecture, not weights] of a classifier, for example to choose the number of hidden units in a neural network. 用来做模型选择

- Test set: A set of examples used only to assess the performance [generalization] of a fully specified classifier. 

The **validation phase** is often split into two parts:

1. In the first part you just look at your models and select the best performing approach using the validation data (=validation)

2. Then you estimate the accuracy of the selected approach (=test).

-- From [here](http://stats.stackexchange.com/questions/19048/what-is-the-difference-between-test-set-and-validation-set)
