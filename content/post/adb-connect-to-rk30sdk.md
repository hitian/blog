---
aliases:
- /notes/2013/02/24/adb-connect-to-rk30sdk/
categories: notes
date: "2013-02-24T00:00:00Z"
tags:
- adb
- android
title: adb连接到rk30sdk
---
修改 `~/.android/adb_usb.ini` 文件

添加

`0x2207`

注意文件中不能有空行, 否则可能报错:

```bash
* daemon not running. starting it now on port 5037 *
ADB server didn't ACK
* failed to start daemon *
```

文件修改好后重启一下服务

```bash
tianMac:platform-tools tian$ ./adb kill-server
tianMac:platform-tools tian$ ./adb start-server
* daemon not running. starting it now on port 5037 *
* daemon started successfully *
```

然后应该就可以看见设备了.
