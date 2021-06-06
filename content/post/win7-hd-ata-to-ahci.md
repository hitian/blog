---
aliases:
- /other/2011/12/03/win7-hd-ata-to-ahci/
categories: other
date: "2011-12-03T00:00:00Z"
tags:
- windows
- ahci
title: win7安装完成后将硬盘从ATA模式切换到AHCI
---

ahci模式包含了ncq等高级模式, 可以提高硬盘的性能, 而且win7内置了驱动, 不必担心像xp会蓝屏.
 
但是如果是在ata模式下安装完成的win7, 再修改bios 切换到ahci模式还是会导致重新启动时蓝屏.
 
解决:
 
在ata模式下进入系统
 
打开注册表编辑器
 
修改 `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Msahci` 中 Start 的值为0 
 
重启电脑 进bios 切换到ahci模式
 
启动系统
 
进入桌面后会显示有驱动正在安装.
 
再手动下载安装一下intel的快速存储技术驱动就好了...
