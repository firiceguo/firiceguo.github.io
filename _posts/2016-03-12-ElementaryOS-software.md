---
layout: post
title: "Elementary OS 探索 —— 基本软件"
date: 2016-03-11
tags: ElementaryOS
categories: ElementaryOS
thumbnail:  linux
description: Elementary OS 的基本软件
toc: true
---

# Elementary OS 探索 —— 基本软件

## 0. Screenshot

在进入系统之后，可以看到，`EOS` 使用了一干小众软件作为其默认软件，不能说不好，但是用户习惯还在，所以需要根据自己的习惯安装软件和主题。闲言少叙，先上图晒桌面：

![screenshot]({{ site.baseurl | prepend:site.url}}/images/screenshot-EOS-1.png){: .center-image }*EOS Screenshot*


## 1. 主题

首先有一个名叫 [Tweaks](http://www.elementaryos-fr.org/documentation/customisation/elementary-tweak/) 的工具，它能帮助用户调整系统，也可以更换系统主题，安装很简单：

{% highlight bash %}
sudo apt-add-repository ppa:mpstark/elementary-tweaks-daily
sudo apt-get update
sudo apt-get install elementary-tweaks
{% endhighlight bash %}

然后就可以在`应用程序`-`系统工具`-`系统设置`-`个人`里面找到它了

作为一个谷歌脑残粉，必然是对 [Material](https://www.google.com/design/spec/material-design/introduction.html) 风格情有独钟的，因此，偶然在网上发现的 [Paper](http://snwh.org/paper/) 主题就很合胃口。安装方式也很简单：

{% highlight bash %}
sudo apt-add-repository ppa:snwh/pulp
sudo apt-get update
sudo apt-get install paper-gtk-theme paper-icon-theme
{% endhighlight bash %}

## 2. 视频播放器

其实自带的视频播放器 `totem` 和整个界面很搭，但是要播放视频还要下载解码器，所以就直接上 `VLC` 了，安装方式：

{% highlight bash %}
sudo add-apt-repository ppa:videolan/stable-daily
sudo apt-get update
sudo apt-get install vlc
{% endhighlight bash %}

之后至于换主题什么的就看个人喜好了，把皮肤文件扔到 `/usr/share/vlc/skins2/` 路径下就可以了。

## 3. 音乐播放器

音乐播放器我用的是 [Tomahawk](https://www.tomahawk-player.org/) ，这是一个跨平台的音乐播放器，安装方式：

{% highlight bash %}
sudo apt-add-repository ppa:paulo-miguel-dias/tomahawk
sudo apt-get update
sudo apt-get install tomahawk
{% endhighlight bash %}

不要问我为什么不用自带的，不能三秒上手的播放器就不是好播放器。

## 4. 浏览器

对于浏览器这种东西，我只能说：Chrome 大法好。这个我是直接官网下deb包搞定的，然后经过一系列配（fan）置（qiang）之后，同步书签和扩展程序还是很顺利的。不得不说，虽然 win10 里的 Edge 速度很快，但是我那么多书签难道每次还要手动同步吗？想屏蔽个广告也不知道怎么弄，一个扩展可以搞定的事情要弄半天，好不容易搞定了，换台机器或者换个系统又要从头再来，这就是天生缺陷，所以说微软还是任重而道远啊。

## 5. 第三方软件

首先推荐一个网站 [Elementary apps](https://quassy.github.io/elementary-apps/) ，这里面有很多优秀的第三方软件，我就从里面下载了几个：

- [Webby](https://quassy.github.io/elementary-apps/Webby/)
- [MarkMyWords Markdown editor](https://quassy.github.io/elementary-apps/MarkMyWords/)
- [NaSC Calculator](https://quassy.github.io/elementary-apps/NaSC/)

其中 Webby 是最有用的，可以把 web 应用弄成 app 样式的，微信，印象笔记，网易云这些就方便点。不过不知道是什么原因，访问速度总是特别慢，这也算是一个bug吧。

然后还有 WPS 也是一个要装的，毕竟要看文件。

## 6. 搜狗拼音输入法

就是它就是它，差点让我重装系统，这是搜狗拼音 for Linux 的[官网](http://pinyin.sogou.com/linux/)。[这里](http://sunshine3.blog.51cto.com/3988340/1693945)是我查到的解决方案：

首先下载安装包，然后：
{% highlight bash %}
sudo vim /etc/os-release #把 ID="elementary OS" 改成 ID="elementaryOS" 即去掉空格
dpkg -i sogoupinyin_2.0.0.0068_amd64.deb
sudo apt-get install -f
{% endhighlight bash %}

然后再改回来就行了

