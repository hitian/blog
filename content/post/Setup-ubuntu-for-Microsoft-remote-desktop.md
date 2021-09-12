---
title: "Setup Ubuntu for Microsoft Remote Desktop"
description: 
date: 2021-09-12T14:15:39+08:00
categories: notes
tags:
- linux
- ubuntu
comments: true
comments: true
draft: false
---

让 ubuntu 20.04 支持 `Microsoft Remote Desktop` 远程桌面连接

### 安装依赖

这里安装了桌面 `xfce4` , 个人感觉ubuntu自带的`GNOME 3`并不是很方便远程桌面使用😶

```bash

sudo apt install xrdp xorg xfce4 xfce4-goodies dbus-x11 x11-xserver-utils xfce4-terminal 

```

### 设置服务和配置

将桌面更改为 `xfce`, 编辑文件 `/etc/xrdp/startwm.sh`

```bash

# 将这两行注释
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession

# 新增
exec startxfce4

```

设置xsession

```bash
echo xfce4-session > ~/.xsession
```

### 设置 xrdp 自启动并重启

```bash
sudo systemctl enable --now xrdp
sudo systemctl restart xrdp
```

## 安装注意

xrdp 默认开放 `3389/TCP` 到所有的网卡, 如果要跨公网使用, 最好使用ufw限制一下可以连接的ip段, 另外修改默认的端口也能减少被试探的概率