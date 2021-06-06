---
aliases:
- /notes/2019/08/25/raspberry-pi-note/
categories: notes
date: "2019-08-25T00:00:00Z"
tags:
- raspberry pi
- raspberry
- pi
title: Raspberry Pi note
---

# Raspberry Pi 笔记

## B系列硬件参数

| Version |         | CPU       |                  |                    | Arch            | RAM       | USB                    | Boot from USB | Ethernet            | Wireless                          | Release      |
|---------|---------|-----------|------------------|--------------------|-----------------|-----------|------------------------|---------------|---------------------|-----------------------------------|--------------|
| 1       | B       | BCM2835   | 700MHz           | 32\-bit            | ARM1176JZFS     | 256/512MB | USB2 \* 2              | No            | 10/100 Mbit/s       | \-                                | 2012 Apr~Jun |
| 1       | B\+     | BCM2835   | 700MHz           | 32\-bit            | ARM1176JZFS     | 256/512MB | USB2 \* 4              | No            | 10/100 Mbit/s       | \-                                | 2014 Jul     |
| 2       | B v1\.1 | BCM2836   | 900MHz           | 32\-bit quad\-core | ARM Cortex\-A7  | 1GB       | USB2 \* 4              | No            | 10/100 Mbit/s       | \-                                | 2015 Feb     |
| 2       | B v1\.2 | BCM2837   | 900MHz ~ 1\.2GHz | 64\-bit quad\-core | ARM Cortex\-A53 | 1GB       | USB2 \* 4              | Yes           | 10/100 Mbit/s       | \-                                | 2016 Oct     |
| 3       | B       | BCM2837   | 1\.2GHz          | 64\-bit quad\-core | ARM Cortex\-A53 | 1GB       | USB2 \* 4              | Yes           | 10/100 Mbit/s       | 2\.4G WIFI \+ Bluetooth 4\.1      | 2016 Feb     |
| 3       | B\+     | BCM2837B0 | 1\.4GHz          | 64\-bit quad\-core | ARM Cortex\-A53 | 1GB       | USB2 \* 4              | Yes           | 10/100/300 Mbit/s   | 2\.4/5G WIFI \+ Bluetooth 4\.2 LS | 2018 Mar     |
| 4       | B       | BCM2711   | 1\.5GHz          | 64\-bit quad\-core | ARM Cortex\-A72 | 1/2/4/8GB | USB2 \* 2 \+ USB3 \* 2 | Yes           | 10/100/1000 Mbit/s  | 2\.4/5G WIFI \+ Bluetooth 5\.0    | 2019 Jun     |

## 常用的系统

### 官方raspbian 系统

[https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

可用于所有型号的raspberrypi

优点

* 基于debian，使用 apt 包管理工具， 经常使用 Ubuntu 的话， 还是比较顺手的
* 包含 raspi-config (Raspberry Pi Software Configuration Tool), 方便调整参数

缺点

* 目前只有 32-bit 的系统(armhf)

`1、2代推荐使用`

### Ubuntu MATE

[https://ubuntu-mate.org/raspberry-pi/](https://ubuntu-mate.org/raspberry-pi/)

Model B 2, 3 or 3+ 适用

优点

* 基于 Ubuntu，使用 apt 包管理工具
* 使用 Ubuntu meta 的桌面，干净，个人偏好

缺点

* 同样只有 32-bit 的系统(armhf)

`推荐需要干净桌面+浏览器的用户使用`

### OSMC

这是一个 `Kodi` 的发行版本，用树莓派将家里的电视变成媒体中心，播放内网下载好的视频，或者安装插件直接播放网络视频

[https://osmc.tv/download/](https://osmc.tv/download/)

可用于所有型号的raspberrypi， 友情提示， 最好在有有线网络接口(USB网卡也可以)的pi上使用， 使用2.4G Wi-Fi播放高清视频绝对会卡的

### Ubuntu Server

作为服务器使用推荐

Ubuntu 20.04 LTS (Focal Fossa)
[http://cdimage.ubuntu.com/releases/20.04/release/](http://cdimage.ubuntu.com/releases/20.04/release/)

[Raspberry Pi Generic (Hard-Float) preinstalled server image](http://cdimage.ubuntu.com/releases/20.04/release/ubuntu-20.04-preinstalled-server-armhf+raspi.img.xz)

For 32-bit

[Raspberry Pi Generic (64-bit ARM) preinstalled server image](http://cdimage.ubuntu.com/releases/20.04/release/ubuntu-20.04-preinstalled-server-arm64+raspi.img.xz)

For 64-bit

## 系统镜像文件写入SD/TF卡

### 简单模式

推荐新手使用`Etcher` [https://github.com/balena-io/etcher/releases](https://github.com/balena-io/etcher/releases)

Windows, linux, macOS 可用

### 命令行模式

`sudo dd if=2019-07-10-raspbian-buster-full.img of=/dev/rdisk2 bs=1m`

linux, macOS 可用，不同系统上目标磁盘的格式可能不太一样，macOS 注意 `/dev/disk2` 和 `/dev/rdisk2` 的区别，`rdisk`写入速度会快10倍以上

注意，dd会覆盖磁盘上的数据，谨慎操作。

### 使用USB硬盘启动

参考官方说明 [https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md)

简单的说就是 3 B+ 及以上硬件版本可以直接从USB设备启动

之前的硬件版本需要手动更改启动模式

## 常用命令

* 系统配置 (官方raspbian 系统可用)

`sudo raspi-config`

* 查看树莓派的型号

`cat /proc/device-tree/model`

* 查看有线网络当前的链接速度 单位mb

`cat /sys/class/net/eth0/speed`

* 查看当前核心温度 (除1000为摄氏度)

`cat /sys/class/thermal/thermal_zone0/temp`
