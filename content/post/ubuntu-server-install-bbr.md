---
aliases:
- /notes/2017/02/07/ubuntu-server-install-bbr/
categories: notes
date: "2017-02-07T00:00:00Z"
tags:
- Ubuntu
- Server
- BBR
title: Ubuntu Server Install BBR
---

首先需要更新到4.9以上的内核版本， ubuntu和debian可以参考 [这里](https://hitian.info/notes/2017/02/07/ubuntu-server-install-mainline-kernel/)

重启确认内核版本升级完成继续

以root用户执行以下命令

<!--more-->

```bash

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

```

检查是否修改成功

```bash
sysctl net.ipv4.tcp_available_congestion_control

#结果如下， 包含bbr
#net.ipv4.tcp_available_congestion_control = bbr cubic reno

lsmod | grep bbr

#结果如下， 包含 tcp_bbr
#tcp_bbr                20480  40

```
