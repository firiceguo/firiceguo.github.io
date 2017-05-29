---
layout: post
title: "Association Rules Mining"
date: 2016-09-11 19:55
tags: DataMining
categories: DataMining
thumbnail:  book
comments: true
description: Algorithms for generating association rules 
---

## 问题描述

给出一些交易的列表，如超市的流水交易单，找出商品之间的相关关系。

### 关联规则

如果顾客经常买`A`商品的同时购买`B`商品，则记为`{A} -> {B}`，即为一种关联规则。

### Support (s)

对于关联规则`{A} -> {B}`来说，`s = (AB同时出现的单数) / (总单数)`

### Confidence (c)

对于关联规则`{A} -> {B}`来说，`c = (AB同时出现的单数) / (A出现的单数)`

### Threshold

找到`s > min_support_threshold`和`c > min_confidence_threshold`的关联规则


## 1. Brute-force (暴力)

- 列举出来所有可能的关联规则

- 对于每个规则计算出所有的s值和c值

- 找到符合`s > min_support_threshold`和`c > min_confidence_threshold`的关联规则

> 暴力解法复杂度太高，不能用
{: .note}


## 2. Apriori Principal

剪枝思路：如果一个规则的出现频率比较小，那么其子规则就可以忽略

> 即：如果{AB}这个规则出现的频率很低，那么{ABC}这个规则出现的频率一定会很低

算法思路：

1. 扫描一遍所有的交易单，记录所有单个产品的出现频数，删掉所有出现频数低于阈值的产品；

2. 列出所有可能的两个产品的候选集，扫描交易单，记录所有两个产品同时出现的频数，删掉所有出现频数低于阈值的产品组合；

3. 列出所有可能的三个产品的候选集，扫描交易单，记录所有三个产品同时出现的频数，找出较大的，即得到基于三个产品的关联规则。

4. 如果需要，可以继续，找到可能的四个产品的候选集。

### Hash Tree

Hash Tree 根据候选集生成的，其目的是减少候选集和交易单比较的次数。

1. 根据所有的候选集建立三叉树，被3除余1的居左，被3除余2的居中，被3除余0的居右，

2. 如果有哪一层的叶子节点数大于3，就继续往下分。

3. 对于每一个交易单，都要过一遍这个树，跟着树分解，如果走到下面发现有和候选集的元素一样的，就对候选集的频数+1。


## FP-Tree

- Step 1:Deduce the ordered frequent items. For items with the same frequency, the order is given by the alphabetical order.（和Apriori Principal的第一步相同，得到所有单个物品的频数，并且删掉比阈值少的部分）

- Step 2:Construct the FP-tree from the above data.（如下图所示）

![image]({{ site.baseurl | prepend:site.url}}/images/FPTree1.png){: .center-image }

- Step 3:From the FP-tree above, construct the FP-conditional tree for each item (or itemset).

- Step 4:Determine the frequent patterns. 

