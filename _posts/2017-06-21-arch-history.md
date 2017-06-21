---
layout: post
title: "Arch Linux 使用杂记"
date: 2017-06-21 14:12
tags:
    - Linux
categories:
    - Linux
comments: true
description: "Arch Linux 使用杂记"
---

## `yaourt -Syyua` 更新问题

bug:

```
clion package(s) failed to install. Check .SRCINFO for mismatching data with PKGBUILD.

```

solve:

```bash
sudo rm -rf /etc/pacman.d/gnupg/
sudo pacman-key --init
sudo pacman-key --populate archlinux antergos
```