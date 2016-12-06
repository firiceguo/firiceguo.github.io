---
layout: post
title: "Distributed TensorFlow"
date: 2016-12-04 21:23
tags:
    - MachineLearning
    - Distributed
    - python
    - web
categories:
    - MachineLearning
    - Distributed
    - python
    - web
thumbnail: tasks
description: Distributed TensorFlow test and simple evaluate
---

## 1. TensorFlow 和 分布式 TensorFlow

废话不多说，[TensorFlow](https://www.tensorflow.org/) 大概是一个以张量流图形式来描述机器学习算法的一个非常强大的开源库，又是谷歌大人开源的，请收下我的膝盖。

[分布式Tensorflow](https://www.tensorflow.org/versions/r0.12/how_tos/distributed/index.html#distributed-tensorflow) 可以将 TensorFlow 运行在 CPU 集群和 GPU 集群上，可以通过 [kubernetes](http://kubernetes.io/) 管理 [docker](https://www.docker.com/) 集群来实现大规模管理。它的文档其实不算很清晰，至少对于我这种新手来说不是很清晰，我还是通过[这篇](https://ischlag.github.io/2016/06/12/async-distributed-tensorflow/)文章来理解的，因此我实验的时候用的方法也是通过参数服务器（Parameter server，简称PS）共享参数，手动跑脚本实现的。

个人理解，通过共享参数的意思就是，所有的worker做完自己的一次迭代之后把更新的参数上传到参数服务器，让别的worker能够使用。这样，如果单机版需要跑1000次迭代，有5台做worker的服务器，那么每台迭代200次就可以达到之前一台机器迭代1000次的效果。

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

这里的 chief 还有一点讲究，官方对于 chief 的解释是这样的：

> Training with replicas you deploy the same program in a Cluster. One of the tasks must be identified as the chief: the task that handles initialization, checkpoints, summaries, and recovery. The other tasks depend on the chief for these services.

所以，被当作 chief 的机器的负载还会重一些。这里还需要注意把原来代码的session交给这个session接管。

这样，最简单的分布式就配好了。其实还有一种方法，也可以让不同的 worker 做不同的事情，在[这里](http://learningtensorflow.com/lesson11/)有教程，我还没有试。

关于运行，可以用下面的脚本分别在两台服务器上面运行：

在 PS & Worker上：

{% highlight bash %}
#!/bin/sh

dir=./log/1

date > $dir/output-0
python $1 --job_name="ps" --task_index=0 >> $dir/output-0 &
python $1 --job_name="worker" --task_index=0 >> $dir/output-0
date >> $dir/output-0

{% endhighlight bash %}

在worker上：

{% highlight bash %}
#!/bin/sh

dir=./log/1

date > $dir/output-1
python $1 --job_name="worker" --task_index=1 >> $dir/output-1
date >> $dir/output-1

{% endhighlight bash %}

## 3. 简要测试

TensorFlow 官方提供了一些 [models](https://github.com/tensorflow/models) 来供学习和测试。我改了 `AutoEncoder` 和 `Transformer` 这两个模型来做测试。

修改过的模型在[这里](https://github.com/firiceguo/my-tensorflow-distributed)。

分别测试了数据量大小不同，模型复杂度大小不同，服务器分工不同时服务器的CPU和网络的情况。

由于图太多就先不放图了，初步结论是，当改变数据量大小的时候，CPU利用率并不会增加，对于某些算法，会延长其运行时间，并改变网络利用情况，因此，算法的不同是主要考虑因素。当模型大小改变的时候，总体来看，会对CPU和网络产生更大的压力。在对集群进行分工的时候，网络传输会成为性能的瓶颈，因此，尽可能减少集群中各个节点的之间的网络传输，即在CPU仍有富裕的情况下，尽可能把PS和Worker分配到一起，可以提高集群的计算效率和CPU使用效率。

## 4. 致谢

42474494264426943963338496863643329264，784726963338496863624853986936944664986246463496898624333743663，28486826364366854，73674693436426。736744329264744969439464269849464986337447464，63496894494。

7848663496882636，96536364484234243248942494364。42694396786968334267364968636。
