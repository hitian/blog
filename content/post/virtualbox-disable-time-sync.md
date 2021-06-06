---
aliases:
- /other/2013/09/20/virtualbox-disable-time-sync/
categories: other
date: "2013-09-20T00:00:00Z"
tags:
- virtualbox
- time
title: virtualbox 禁止时间同步
---
virtualbox 会自动同步虚拟机的时间, 看起来很方便, 却给开发造成了不小的麻烦.

<!--more-->

cmd进入virtualbox的目录 ,一般在

`C:\Program Files\Oracle\VirtualBox\`

网上说命令的格式为

`vboxmanage setextradata <vmname> "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled" "1"`

看了下说明. 如果懒得去找 <vmname>, 可以使用global 直接修改全局的配置

执行

```./vboxmanage.exe setextradata global "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled" "1"```

然后重启虚拟机实测对guest是linux 的有效
