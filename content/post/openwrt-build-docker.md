---
aliases:
- /notes/2019/08/10/openwrt-build-docker/
categories: notes
date: "2019-08-10T00:00:00Z"
tags:
- docker
- ubuntu
- openwrt
title: 在docker中编译openwrt
---

# 在docker中编译openwrt

## build docker image

Docker file

{% gist 88ff8a7cad5deffad51d8c29d5c47d75 Dockerfile %}

`docker build --no-cache -t ubuntu-openwrt-build .`

## build

clone openwrt source code

`git clone https://git.openwrt.org/openwrt/openwrt.git`

```shell

# 更新软件包

docker run -it --rm -v $PWD:/build ubuntu-openwrt-build ./scripts/feeds update -a

# 使用 menuconfig 设置编译目标、内核信息、以及需要安装的包

docker run -it --rm -v $PWD:/build ubuntu-openwrt-build make menuconfig

# 编译

docker run -it --rm -v $PWD:/build ubuntu-openwrt-build make -j4

```

![menuconfig](/assets/images/201908/docker-openwrt-build-menuconfig.png)

生成的文件会放置在`bin`目录下, 例如 MT7621 放在 `bin/targets/ramips/mt7621`

```shell

#配置文件，同运行 make menuconfig 后生成的.config 文件，可以用于备份和还原配置
config.seed
openwrt-ramips-mt7621-device-pbr-m1-t.manifest
openwrt-ramips-mt7621-pbr-m1-t-initramfs-kernel.bin #首次刷机使用
openwrt-ramips-mt7621-pbr-m1-t-squashfs-sysupgrade.bin #更新使用
packages  #编译好的设置为编译但是不安装的包

```

## 添加新设备

示例 [https://github.com/hitian/openwrt/commit/22df459310836b92caa8fb1796885f670214d25a#diff-8dabb1882176c362bafecc664a7af2ee](https://github.com/hitian/openwrt/commit/22df459310836b92caa8fb1796885f670214d25a#diff-8dabb1882176c362bafecc664a7af2ee)
