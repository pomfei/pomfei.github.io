---
date: 2023-12-22 22:29:43 +0800
layout: post
title: Jekyll因为时区问题无法显示文章的解决办法
categories: Tips
tags: 新奇好玩
---

刚写好的文章push到GitHub上无法显示，Google了一下发现是因为时区的问题导致新提交的文章没有被编译。

解决的办法如下：

1、在_config.yml中添加时区设定

```
timezone: Asia/Shanghai
```

2、在文章头部的时间戳位置添加 +0800

```
date: 2023-12-22 22:29:43 +0800
```

参考链接：

<https://changwh.github.io/2019/03/17/timezone-issue-in-jekyll/>