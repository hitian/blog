---
aliases:
- /notes/2017/01/17/google-compute-engine-simple-test/
categories: notes
date: "2017-01-17T00:00:00Z"
tags:
- Google
- Compute
- Engine
- Test
- Cloud
title: Google Compute Engine 简单测试
---
这几天在试用Google Cloud Platform，觉得Compute Engine的运行速度貌似挺慢的，就测试了一下

选了距离比较近的区域 asia-east1-a, 貌似是在台湾的。

创建了3个不同配置的实例

1. f1-micro（1 个 vCPU，0.6 GB 内存） `$5.00/月`
2. g1-small（1 个 vCPU，1.7 GB 内存） `$15.73/月`
3. n1-standard-1（1 个 vCPU，3.75 GB 内存）`$28.50/月`

系统都选择了 `Ubuntu 16.04.1 LTS`


#### CPU测试

测试方式 Node.js v7.4.0 源代码编译 (时间统计方式 `time make`)

1. f1: 155m35.651s （两个多小时。。）
2. g1: 55m9.996s
3. n1: 28m27.362s

相比之下

* digitalocean 	`$5.00/月` 的最低配用时 43m11.467s
* 本地的笔记本i7-5557m的CPU使用2个核心 27m47.806s

另外f1和g1是共享cpu的
![GCE-cpu-Screen-Shot](/assets/images/201701/GCE-Screen-Shot-2017-01-14-at-09.57.07.png)

可以看到在cpu负载上升后可以超过限制运行短暂的时间。

#### 然后测试一下磁盘的IO

测试方式 直接使用 `dd if=/dev/zero of=/var/swap.img bs=1024k count=1000` 创建一个1G的大文件

1. f1: 1048576000 bytes (1.0 GB, 1000 MiB) copied, 25.6226 s, 40.9 MB/s
2. g1: 1048576000 bytes (1.0 GB, 1000 MiB) copied, 21.2997 s, 49.2 MB/s
3. n1: 1048576000 bytes (1.0 GB, 1000 MiB) copied, 4.83823 s, 217 MB/s

可以因为选择的是`新的 10 GB 标准永久性磁盘`， 速度感觉一般般了

相比之下digitalocean因为用的SSD, 速度快了不止一点
oc: 1048576000 bytes (1.0 GB, 1000 MiB) copied, 1.71034 s, 613 MB/s


从简单的测试数据来看， 很明显如果是平时用来折腾的vps的话GCE性价比太低了。而且不知道为什么，GCE创建一个实例差不多要耗时2分钟左右😂

##### ps

f1 默认的0.6G内存编译Node.js竟然会不够用。加了虚拟内存之后才顺利的编译完成

Ubuntu 添加swap:

`dd if=/dev/zero of=/var/swap.img bs=1024k count=1000`

`mkswap /var/swap.img`

`swapon /var/swap.img`


