---
layout: post
title: "Cluster Analysis: Basic Concepts and Methods"
date: 2016-10-07 11:55
tags: DataMining
categories: DataMining
thumbnail:  book
comments: true
description: Cluster Analysis - Basic Concepts and Methods 
---

## 聚类算法（Cluster Analysis）

- 目的：只把相似的归为一类；

- Unsupervised learning

## 主要聚类方法

1. 划分（Partitioning approach）：先建立区块，之后根据某些原则进行评估。常用方法：k-means, k-medoids, CLARANS

2. 分级（Hierarchical approach）：常用方法：Diana, Agnes, BIRCH, CAMELEON

3. 基于密度（Density-based approach）：通过连通性和密度函数分类。常用方法：DBSACN, OPTICS, DenClue

4. 基于栅格（Grid-based approach）：常用方法：STING, WaveCluster, CLIQUE

5. 基于模型（Model-based）：常用方法：EM, SOM, COBWEB

6. 基于频繁模式（Frequent pattern-based）：常用方法：p-Claster

7. User-guided or constraint-based：常用方法：COD (obstacles), constrained clustering

8. 基于连接（Link-based clustering）：常用方法：SimRank, LinkClus

## 聚类的度量指标

### 外部指标（与某个“参考模型”进行比较）

结果均在`[0, 1]`之间，值越大越好。

- Jaccard 系数：$$JC = \frac a {a+b+c}$$

- FM 指数：$$FMI = \sqrt{{\frac a {a+b}}*{\frac a {a+c}}}$$

- Rand 指数：$$RI = \frac {2(a+d)}{m(m-1)}$$

对数据集$$D={x_1, x_2, ... , x_m}$$，假定通过聚类给出的簇划分为$$C = {C_1, C_2, ..., C_k}$$，参考模型给出的簇划分为$$C^* = {C^*_1, C^*_2, ..., C^*_s}$$。相应地，令$$\lambda$$与$$\lambda^*$$分别表示与$$C$$和$$C^*$$对应的簇标记向量。将样本两两配对考虑，定义：

$$a = |SS|, SS = {(x_i, x_j)|\lambda_i=\lambda_j, \lambda^*_i=\lambda^*_j, i<j}$$

$$b = |SD|, SD = {(x_i, x_j)|\lambda_i=\lambda_j, \lambda^*_i \neq \lambda^*_j, i<j}$$

$$c = |DS|, DS = {(x_i, x_j)|\lambda_i \neq \lambda_j, \lambda^*_i=\lambda^*_j, i<j}$$

$$d = |DD|, SS = {(x_i, x_j)|\lambda_i \neq \lambda_j, \lambda^*_i \neq \lambda^*_j, i<j}$$

### 内部指标（直接考察聚类结果，不利用参考模型）

考虑聚类结果的簇划分：$$C = {C_1, C_2, ..., C_k}$$，定义：

簇$$C$$内样本间的平均距离：$$avg(C) = \frac 2 {|C|(|C|-1)} \sum_{1 \leq i < j \leq |C|}dist(x_i, x_j)$$

簇$$C$$内样本间的最远距离：$$diam(C) = max_{1 \leq i < j \leq |C|}dist(x_i, x_j)$$

簇$$C_i$$与簇$$C_j$$最近样本的距离：$$d_{min}(C_i, C_j) = min_{x_i \in C_i, x_j \in C_j}dist(x_i, x_j)$$

簇$$C_i$$与簇$$C_j$$中心点的距离：$$d_{cen}(C_i, C_j) = dist(\mu_i, \mu_j)$$

基于上面四个式子导出下面常用的内部指标：

- DB指数(越小越好)：$$DBI = \frac 1 k \sum_{i=1}^k max_{j \neq i}(\frac {avg(C_i) + avg(C_j)}{d_{cen}(\mu_i, \mu_j)})$$

- Dunn指数(越大越好)：$$DI = min_{1 \leq i \leq k} \left\{ min_{j\neq i}(\frac {d_{min}(C_i, C_j)}{max_{1\leq l\leq k}diam(C_l)})\right\}$$

## Partitioning method

把拥有`n`个对象的数据集`D`分成`k`类，目的是最小化平方距离和`E`：

$$ E = \sum^k_{i=1} \sum_{p \in C_i} (d(p,c_i)^2) $$

> k类的k的取值不定，需要自己设定。

### K-Means

1. 首先将对象分为`k`个非空子集中；

2. 计算当前分类的每类的中点（中点指中心，比如集群的平均点），当作种子点；

3. 把每个对象分配给最近的种子点代表的类中；

4. 返回第二步重新计算，直到分配不会再变为止。

时间复杂度：`O(tkn)`，其中n是对象个数，k是类的个数，t是迭代次数。比`PAM`算法的时间复杂度$$ O(k(n-k)^2) $$和`CLARA`算法的时间复杂度$$O(ks^2+k(n-k))$$低。

缺点：只能将连续n维空间的对象分类；需要设定k；对噪声很敏感；无法检测非凸形状。

### K-Medoids（PAM）

1. 随机选择k个对象作为初始medoids；

2. 把剩下的对象依据距离这k个对象的距离进行分类，并计算距离花费之和；

3. 随机选择一个非medoid点；

4. 将刚才的点替代初始medoid点，计算此时的距离花费之和；

5. 如果距离花费变少了，就替换medoid点；如果变多了，就不替换；然后跳转到第二步

- CLARA(Clustering Large Applications)：PAM on samples

- CLARANS：Randomized re-sampling

## Hierarchical Clustering

使用距离作为分类的标准，这个算法并不要求k的大小作为输入，但是要有一个终止条件。通常使用树状图，其中聚类算法是`AGNES`，分裂算法是`DIANA`。

### 类间距

- Single link：集群中最近两个元素的距离 $$ \text{dist}(K_i,K_j)=\min(t_{ip},t_{jq}) $$

- Complete link：集群中最远两个元素的距离 $$ \text{dist}(K_i,K_j)=\max(t_{ip},t_{jq}) $$

- Average：集群中所有元素距离的平均值 $$ \text{dist}(K_i,K_j)= \text{avg}(t_{ip},t_{jq}) $$

- Centroid：两个集群的中心距离 $$ \text{dist}(K_i,K_j)= \text{dist}(C_i,C_j) $$

- Medoid：两个集群的medoid之间的距离 $$ \text{dist}(K_i,K_j)= \text{dist}(M_i,M_j) $$

### 中点(Centroid) 直径(Diameter) 半径(Radius)

- Centroid：$$ C_m=\frac{\sum^N_{i=1}(t_{ip})}{N} $$

- Diameter：$$ D_m=\sqrt{\frac{\sum^N_{i=1}\sum^N_{i=1}(t_{ip}-c_m)^2}{N(N-1)}} $$

- Radius：$$ R_m=\sqrt{\frac{\sum^N_{i=1}(t_{ip}-c_m)^2}{N}} $$

## Density-Based Methods (DBSCAN)

基于密度的方法可以对任意形状起作用，对隔离噪声的能力强，只需要扫描一遍，但是需要密度参数作为终止条件。

需要两个参数：

- Eps：最大半径。只有距离大于最大半径的点不算临近点，只有距离小于这个值才算临近点。

- MinPts：最少点数。只有在Eps之内的点数多于MinPts才能算这个类中的点，否则这个点被视为边界或者噪声。

## Grid-Based Methods

采用多分辨率的网格数据结构，大致是通过将整个区域划分成栅格（可能有多层），然后进行分类。
