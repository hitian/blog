---
aliases:
- /notes/2017/02/07/ubuntu-server-install-mainline-kernel/
categories: notes
date: "2017-02-07T00:00:00Z"
tags:
- Ubuntu
- Server
- Mainline
- Kernel
title: Ubuntu Server Install Mainline Kernel
---

### 下载最新的内核

<a href="http://kernel.ubuntu.com/~kernel-ppa/mainline/?C=N;O=D" target="_blank">http://kernel.ubuntu.com/~kernel-ppa/mainline/?C=N;O=D</a>

进入最新的版本目录

需要下载4个文件

 * linux-headers-XXXXX_XXXXX_all.deb
 * linux-headers-XXXXX-generic_XXXXX_PLATFORM.deb
 * linux-image-unsigned-XXXXX-generic_XXXXX_PLATFORM.deb
 * linux-modules-XXXXX-generic_XXXXX_PLATFORM.deb

例如目前最新的是v4.17.5 (2018-07-09), 下载这4个文件

 * linux-headers-4.17.5-041705_4.17.5-041705.201807081431_all.deb
 * linux-headers-4.17.5-041705-generic_4.17.5-041705.201807081431_amd64.deb
 * linux-image-unsigned-4.17.5-041705-generic_4.17.5-041705.201807081431_amd64.deb
 * linux-modules-4.17.5-041705-generic_4.17.5-041705.201807081431_amd64.deb

### 安装

```bash

dpkg -i *.deb

```

如果安装报错提示`libssl1.1`没有安装

```
Unpacking openssl (1.1.0g-2ubuntu5) over (1.0.2g-1ubuntu4.13) ...
dpkg: dependency problems prevent configuration of openssl:
 openssl depends on libssl1.1 (>= 1.1.0); however:
  Package libssl1.1 is not installed.

dpkg: error processing package openssl (--install):
 dependency problems - leaving unconfigured
```

可以从[http://security.ubuntu.com/ubuntu/pool/main/o/openssl/](http://security.ubuntu.com/ubuntu/pool/main/o/openssl/) 下载安装

注意: 如果使用的是linode， 还需要在后台调整 Configuration Profile > Boot Settings > Kernel 为 `GRUB 2` 才能启动到更新后的内核。

### 安装完成后重启一下

确认一下内核是否升级成功

```bash
uname -a
```

