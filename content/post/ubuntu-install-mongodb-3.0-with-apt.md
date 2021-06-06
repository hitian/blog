---
aliases:
- /notes/2015/07/11/ubuntu-install-mongodb-3.0-with-apt/
categories: notes
date: "2015-07-11T00:00:00Z"
tags:
- ubuntu
- mongodb
- 3
- 加速
- aliyun
title: ubuntu 通过apt 安装 mongodb 3.0
---

ubuntu version:

<!--more-->

```bash
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=15.04
DISTRIB_CODENAME=vivid
DISTRIB_DESCRIPTION="Ubuntu 15.04"
```



新建文件 `/etc/apt/sources.list.d/mongodb-org-3.0.list` 内容如下 (使用aliyun的mirror进行加速)

```bash
deb http://mirrors.aliyun.com/mongodb/apt/ubuntu/ trusty/mongodb-org/3.0 multiverse
```


添加key & update apt:

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
sudo apt-get update
```


安装 mongodb 最新版本

```bash
sudo apt-get install mongodb-org
```

目前安装的版本
```bash
~$ mongo --version
MongoDB shell version: 3.0.4
```
