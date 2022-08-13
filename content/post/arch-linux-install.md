---
title: "arch linux install note"
description: 
date: 2022-08-13T13:44:43+08:00
categories: notes
tags:
- linux
---

arch linux install note

# Pre-installation

 verify image signature

`gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig`

## Boot Option

before boot up, update BIOS Boot Options to EFI

## Install

### partition disk

list disks `fdisk -l`

use fdisk to modify disk partition tables

`fdisk /dev/sda`

![archlinux-fdisk](/assets/images/202208/archlinux-fdisk.png)

example: 
+ 500 MiB for EFI system partition
+ 2048 MiB for Linux swap
+ remainder for Linux root partition

step:

```bash
g # create a new empty GPT partition table
n # create 1st partition
1 # default[1] first partition
 # default[2048] first sector
+500M # first partition size
n # create 2nd partition
2 # default[2]
 # default first sector
+2048M # 2nd partition size
n # create root partition
 # default[3]
 # default first sector
 # default size
t # change partition type
1 # first partition
1 # partition type or alias, type 1: EFI System
t # change partition type
2 # 2nd partition
19 # partition type or alias, type 19:Linux swap
w # save
```

check partition

`fdisk -l`

### format partition

1. first partition EFI system partition to FAT32

`mkfs.fat -F 32 /dev/sda1`

2. 2nd partition swap to swap

`mkswap /dev/sda2`

3. 3rd partition root to ext4

`mkfs.ext4 /dev/sda3`


![archlinux-partition_format.png](/assets/images/202208/archlinux-partition_format.png)

### Mount disk

```bash
# mount root partition to /mnt
mount /dev/sda3 /mnt
# mount EFI partition to /mnt/boot
mount --mkdir /dev/sda1 /mnt/boot
#create swap
swapon /dev/sda2

```
### Install

#### change download mirrors for Mainland China user

edit `/etc/pacman.d/mirrorlist`

add
```plain
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```
to the top of Server list

![archlinux-package-mirror.png](/assets/images/202208/archlinux-package-mirror.png)

update cache

`pacman -Syy`

##### install linux base

`pacstrap /mnt base linux linux-firmware`

#### Configure

```bash
# generate fstab
genfstab -U /mnt >> /mnt/etc/fstab
# chroot
arch-chroot /mnt
# set timezone
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# install package
pacman -S nano netctl dhcpcd
# Localization
echo "en_US.UTF-8 UTF-8" | tee -a /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
# hostname
echo "archlinux" > /etc/hostname
# set root's password
passwd
```

#### Boot loader

```bash
# install package grub and efibootmgr
pacman -S grub efibootmgr
mkdir /boot/grub
grub-mkconfig > /boot/grub/grub.cfg
# install grub
grub-install --efi-directory=/boot --target=x86_64-efi /dev/sda
# reboot to archlinux
reboot
```

## setup network

### Static IP Address using `netctl`

#### check network devices

`ip link`

```plain
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:19:2b:bf brd ff:ff:ff:ff:ff:ff
    altname enp2s1
```

network device name is `ens33`

#### configure

copy example file to etc

`cp /etc/netctl/examples/ethernet-static /etc/netctl/ens33`

edit `/etc/netctl/ens33`

```plain
Description='A basic static ethernet connection'
Interface=eth0
Connection=ethernet
IP=static
Address=('192.168.1.23/24' '192.168.1.87/24')
#Routes=('192.168.0.0/24 via 192.168.1.2')
Gateway='192.168.1.1'
DNS=('192.168.1.1')

## For IPv6 autoconfiguration
#IP6=stateless

## For IPv6 static address configuration
#IP6=static
#Address6=('1234:5678:9abc:def::1/64' '1234:3456::123/96')
#Routes6=('abcd::1234')
#Gateway6='1234:0:123::abcd'
```

stop dhcp client

`systemctl disable --now dhcpcd`

setup auto start

`netctl enable ens33`

start

`netctl start ens33`


### Dynamic IP Address

copy example configure file

`cp /etc/netctl/examples/ethernet-dhcp /etc/netctl/enp2s1`

```plain
Description='A basic dhcp ethernet connection'
Interface=enp2s1
Connection=ethernet
IP=dhcp
#DHCPClient=dhcpcd
#DHCPReleaseOnStop=no
## for DHCPv6
#IP6=dhcp
#DHCP6Client=dhclient
## for IPv6 autoconfiguration
#IP6=stateless

```

enable and start dhcp client

`systemctl enable --now dhcpcd`


## Post-install

### add user

```bash
useradd --create-home tian
```

### add user to sudoers

```bash
usermod -aG wheel tian
# usermod --append --groups wheel tian
```

update sudoers file

```bash
pacman -S sudo
visudo
```

uncomment the following line

`%wheel ALL=(ALL) ALL`

remove from sudoers

```bash
gpasswd -d tian wheel
```

### other package for manage

```bash

pacman -S net-tools dnsutils

```