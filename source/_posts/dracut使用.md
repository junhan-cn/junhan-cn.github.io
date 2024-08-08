---
title: dracut使用
date: 2024-08-08 09:52:41
tags: dracut 系统层
categories: 操作系统
---

# 基本信息
- 内核源码：linux-6.10.2
- 代码阅读工具：vim+ctags+cscope
- os版本： openEuler-22.03-SP3

# dracut介绍
dracut 是基于事件的驱动的initramfs的工具。dracut通过从一个已经安装的系统中复制相应的文件从而创建一个initramfs。从实现上来说dracut 主要是通过一堆脚本定义各个事件。最后通过dracut去调用相应的脚本，复制各个组件，组成initramfs。脚本一般定义在/usr/lib/dracut/modules.d/目录下。日常使用的情况下，目前来看不需要动系统自带的脚本。

<!--more-->
# 使用场景
## 服务器中的使用
- 添加指定驱动到initramfs中 dracut提供了 --add-drivers --force-drivers --omit-drivers 相关的命令去指定是否将相关驱动做到initramfs中。也可以定义相关配置文件，去持久化
  ```
  /etc/dracut.conf
  /etc/dracut.conf.d/ 
  ```
- initramfs被损坏后，通过dracut恢复
initramfs被损坏后，系统无法进入。此时可以通过挂载iso 进入系统去重做initramfs 
使用方法
```
# 挂载相应的分区
mount /dev/vda2 /mnt    //我的环境 vda2是根分区
mount /dev/vda1 /mnt/boot  //vda1是boot分区
mount --rbind /proc /mnt/proc
mount --rbind /dev /mnt/dev
mount --rbind /sys /mnt/sys

chroot /mnt

dracut -f -N --kver <kernel-version>  initramfs-<kernel-version>.img 
```
## 嵌入式中的使用
这部分工作我具体没参与过，当前我能想到的场景就是裁剪initramfs，减少initramfs的大小。主要应对嵌入式的硬件情况。（当然很多嵌入式可以直接 使用busybox）

# 持续更新