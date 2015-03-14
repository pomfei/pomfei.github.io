---
date: 2015-02-25 07:38:43
layout: post
title: 我的Home server
categories: 文章
tags: 新奇好玩
---

**锲子**

家里只有一台RMBP，所以在存储方面经常捉襟见肘，移动硬盘500G已经成为了小容量。下载的电影，存储的资料，孩子的照片视频神马的越来越多，于是就萌发了搞一个家庭存储server这样的想法。

最开始的想法是直接组一台htpc，低功耗的Atom平台，后来通过查找资料发现单纯的diy费时费力，而且还要明确各种


**硬件**


**软件**

下载镜像
http://wiki.smartos.org/display/DOC/Download+SmartOS

烧写SmartOS的img到U盘
https://wiki.smartos.org/display/DOC/Creating+a+SmartOS+Bootable+USB+Key

安装FreeNas提供zfs功能，关于zfs请移步 http://zh.wikipedia.org/wiki/ZFS

在最开始的时候将所有的数据盘初始化到globle zone里面，我没有修改它的名字，在我这里这个zone的名字叫"zones",
下面的命令会将ssd作为cache加入到zone里面。我的ssd硬盘在系统里面是c1t4d0,因为我将它插在sata5的位置。

```shell
zpool add zones cache c1t4d0
```

Install the project FIFO
http://www.mydoop.com/2014/06/vmware-%E5%AE%89%E8%A3%85-smartos-project-fifo-%E4%BA%91%E4%B8%BB%E6%9C%BA/

BTW: 我没有安装成功，不知道为什么会出现time out的错误，于是就放弃了。后来打听到FIFO只是用来管理VM虚拟机的，感觉没有什么必要。

设置Samba共享
参考http://www.perkin.org.uk/posts/setting-up-samba-on-smartos.html

安装windows 7虚拟机

**其他**

To be continued... ...