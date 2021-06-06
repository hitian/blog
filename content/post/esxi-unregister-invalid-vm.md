---
aliases:
- /notes/2017/11/05/esxi-unregister-invalid-vm/
categories: notes
date: "2017-11-05T00:00:00Z"
tags:
- vmware
- esxi
- vm
title: vmware esxi unregister invalid vm
---

*** 以下内容针对 VMware ESXi 6.5.0 build-6765664 ***

意外删除了esxi 的一个vmfs所在的磁盘，导致vm列表下一直会显示一个invalid状态的VM， 无法进行任何操作，看着非常碍眼。显然通过web界面可能无法处理这个问题了

开启SSH服务，登录esxi。

```bash

[root@localhost:/vmfs] vim-cmd vmsvc/getallvms
Skipping invalid VM '6'
Vmid          Name                               File                             Guest OS       Version   Annotation
1      ubuntu-desktop       [HDD] ubuntu-desktop/ubuntu-desktop.vmx           ubuntu64Guest      vmx-13
11     ubuntu-server-1710   [HDD] ubuntu-server-1710/ubuntu-server-1710.vmx   ubuntu64Guest      vmx-13
2      ubuntu-server        [HDD] ubuntu-server/ubuntu-server.vmx             ubuntu64Guest      vmx-13
3      windows-7            [HDD] windows-7/windows-7.vmx                     windows7_64Guest   vmx-13

```

使用 `vim-cmd vmsvc/getallvms` 命令可以看到提示出现问题的是 VM 6， 接下来直接删除就好了。

```bash

[root@localhost:/vmfs] vim-cmd vmsvc/unregister 6

```

刷新web界面， 就可以看到出错的vm已经被删除了。

