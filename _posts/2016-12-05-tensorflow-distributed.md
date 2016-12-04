---
layout: post
title: "Distributed TensorFlow"
date: 2016-12-04 21:23
tags:
    - MachineLearning
    - Distributed
    - Python
categories:
    - MachineLearning
    - Distributed
    - Python
thumbnail:  cubes
description: Distributed TensorFlow test and simple evaluate
---

## 1. TensorFlow 和 分布式 TensorFlow

废话不多说，[TensorFlow](https://www.tensorflow.org/) 大概是一个以张量流图形式来描述机器学习算法的一个非常强大的开源库，又是谷歌大人开源的，请收下我的膝盖。

[分布式Tensorflow](https://www.tensorflow.org/versions/r0.12/how_tos/distributed/index.html#distributed-tensorflow) 可以将 TensorFlow 运行在 CPU 集群和 GPU 集群上，可以通过 [kubernetes](http://kubernetes.io/) 管理 [docker](https://www.docker.com/) 集群来实现大规模管理。它的文档其实不算很清晰，至少对于我这种新手来说不是很清晰，我还是通过[这篇](https://ischlag.github.io/2016/06/12/async-distributed-tensorflow/)文章来理解的，因此我实验的时候用的方法也是通过参数服务器（Parameter server，简称PS）共享参数，手动跑脚本实现的。

## 2. 实现方法

- 首先在原有代码中设置 `PS` 和 `worker`，然后设置cluster，然后设置flag的配置方法，最后开启一个server：

{% highlight python %}
parameter_servers = ["192.168.122.100:2223"]
workers = ["192.168.122.100:2222",
           "192.168.122.101:2222"]
cluster = tf.train.ClusterSpec({"ps": parameter_servers, "worker": workers})

# input flags
tf.app.flags.DEFINE_string("job_name", "", "Either 'ps' or 'worker'")
tf.app.flags.DEFINE_integer("task_index", 0, "Index of task within the job")
FLAGS = tf.app.flags.FLAGS

# start a server for a specific task
server = tf.train.Server(
    cluster, job_name=FLAGS.job_name, task_index=FLAGS.task_index)
{% endhighlight python %}

- 然后加这么一段，意思就是当这个服务是`PS`的时候，加入server中并等待，如果是`worker`，那么开始执行下面的语句：

{% highlight python %}
if FLAGS.job_name == "ps":
    server.join()
elif FLAGS.job_name == "worker":

    with tf.device(tf.train.replica_device_setter(
            worker_device="/job:worker/task:%d" % FLAGS.task_index,
            cluster=cluster)):
{% endhighlight python %}

- 之后就是配置流图，配置结构，完了之后用`initialize_all_variables`初始化所有变量，之后配置一个 Supervisor 并在所有的 worker 加入之后启动 session：

{% highlight python %}
    sv = tf.train.Supervisor(is_chief=(FLAGS.task_index == 0),
                             init_op=init_op)
    with sv.prepare_or_wait_for_session(server.target) as sess:
		.....
{% endhighlight python %}

这样，最简单的分布式就配好了。其实还有一种方法，也可以让不同的 worker 做不同的事情，在[这里](http://learningtensorflow.com/lesson11/)有教程，我还没有试。

## 3. 简要测试

TensorFlow 官方提供了一些 [models](https://github.com/tensorflow/models) 来供学习和测试。我改了 `AutoEncoder` 和 `Transformer` 这两个模型来做测试。

修改过的模型在[这里](https://github.com/firiceguo/my-tensorflow-distributed)。

分别测试了数据量大小不同，模型复杂度大小不同，服务器分工不同时服务器的CPU和网络的情况。

由于图太多就先不放图了，初步结论是，当改变数据量大小的时候，CPU利用率并不会增加，对于某些算法，会延长其运行时间，并改变网络利用情况，因此，算法的不同是主要考虑因素。当模型大小改变的时候，总体来看，会对CPU和网络产生更大的压力。在对集群进行分工的时候，网络传输会成为性能的瓶颈，因此，尽可能减少集群中各个节点的之间的网络传输，即在CPU仍有富裕的情况下，尽可能把PS和Worker分配到一起，可以提高集群的计算效率和CPU使用效率。
