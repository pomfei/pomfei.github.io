---
date: 2024-5-15 16:50:00 +0800
layout: post
title: Fio使用SPDK测试
categories: 学海无涯
tags: 存储
---

一、编译Fio

```shell
./configure
make
```



二、编译SPDK

```shell
./configure --with-fio=/path of fio repo/   #这里要把fio的路径输入
# 可以根据编译需要开启/关闭一些feature
#./configure --disable-unit-tests --enable-debug --without-ocf --without-vhost --without-crypto --without-isal --without-reduce --disable-tests --without-virtio --with-fio=/path of fio repo/   #这里要把fio的地址输入
make(如果编译失败，安装所需依赖后需要make clean后再make)
```

编译成功后会在build/fio/目录下生成spdk_bdev  spdk_nvme这两个文件，后面Fio的Ioengine就可以选择其中一个

三、Fio job配置

```
[global]
ioengine=/spdk path/build/fio/spdk_bdev
filename=trtype=PCIe traddr=0000.06.00.0 ns=1
direct=1
thread=1
buffered=0
size=100%
randrepeat=0
time_based
norandommap
runtime=100

[test]
stonewall
bs=4k
numjobs=1
rw=randread
iodepth=1

```



四、Fio使用

```
LD_PRELOAD=/spdk path/build/fio/spdk_nvme fio  fio.job
```



参考:

https://mp.weixin.qq.com/s/w0U85HaKSZ4DHfYfVd1J8g
