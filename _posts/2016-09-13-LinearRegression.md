---
layout: post
title: "Linear Regression and Logistic Regression"
date: 2016-09-13 18:17
tags: MachineLearning
description: Questions about linear regression and logistic regression 
---

## About Linear Regression

### **什么是线性回归？**

对于给定一个数据集，线性回归试图建立一个线性模型来对这个数据集进行准确的预测。

### **什么是线性模型？**

![chart]({{ site.baseurl | prepend:site.url}}/images/LinearModel.png){: .center-image }

其中`w`是各个属性的权值，`x`则是各个属性的取值。

### **如何度量模型的准确度？**

规定一个误差函数`L`，取值越小越准确。一般用均方误差函数作为误差函数。

> 均方误差的结果有着很好的几何意义，它对应了几何中的“欧氏距离”(Euclidean distance)
{: .note}

### **如何建立线性模型？**

建立线性模型的过程也就是对`w`和`b`两个参数进行估计的过程。目的是使误差函数最小。结果如下：

![chart]({{ site.baseurl | prepend:site.url}}/images/LinearModelSol.png){: .center-image }

> 当 X^{T}X 满秩的时候，可以有唯一解。当其不是满秩的时候（如变量数目大于样例数目时），w就不是唯一解，通常引入正则化（regularization）项来解决这个问题
{: .note}


## About Logistic Regression

### **为什么要引入Sigmoid函数？**

当我们进行二分类任务时，需要将输出标记为0或者1。理想的分类函数是单位阶跃函数，但是其在0处不可导，会为计算增加很多困难。因此引入一种Sigmoid函数——对数几率函数来替代单位阶跃函数来进行分类：

![chart]({{ site.baseurl | prepend:site.url}}/images/sigmoid.png){: .center-image }

![chart]({{ site.baseurl | prepend:site.url}}/images/sigmoid_bipolar.png){: .center-image }

### **如何确定此时的w？**

使用*最大似然法*

