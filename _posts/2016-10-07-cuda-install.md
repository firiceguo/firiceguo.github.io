---
layout: post
title: "CUDA fix bugs"
date: 2016-10-07 19:55
tags: ParallelProgrammin
categories: ParallelProgramming
thumbnail:  calculator
description: Some issuses during using CUDA
---

> 这篇文章记录用于记录我在使用和配置`CUDA`的踩的坑和解决办法们。

## 安装

按照[CUDA官网](https://developer.nvidia.com/cuda-downloads)和[CUDA Quick Start Guide](https://developer.nvidia.com/compute/cuda/8.0/prod/docs/sidebar/CUDA_Quick_Start_Guide-pdf)下载安装。

在
{% highlight bash %}
cd ~/NVIDIA_CUDA-8.0_Samples/5_Simulations/nbody
make
{% endhighlight bash %}
之后报错：
{% highlight bash %}
>>> WARNING - libGL.so not found, refer to CUDA Getting Started Guide for how to find and install them. <<<
>>> WARNING - libGLU.so not found, refer to CUDA Getting Started Guide for how to find and install them. <<<
>>> WARNING - libX11.so not found, refer to CUDA Getting Started Guide for how to find and install them. <<<
{% endhighlight bash %}

搜索之后解决方法为：
{% highlight bash %}
GLPATH=/usr/lib make
{% endhighlight bash %}

但是我想不能每次都加这么一句，我也记不住，所以又加了
{% highlight bash %}
export GLPATH=/usr/lib
{% endhighlight bash %}
这么一句，之后就不会报错了

