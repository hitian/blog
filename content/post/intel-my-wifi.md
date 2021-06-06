---
aliases:
- /other/2011/09/14/intel-my-wifi/
categories:
- other
category_backup: other
date: "2011-09-14T00:00:00Z"
tags:
- intel
- wifi
title: intel my wifi 无法获取到ip的解决
---
平时都使用脚本来共享wifi给手机

start_wifi.bat>
```bat
netsh wlan set hostednetwork mode=allow ssid=TIANap key=21345678
netsh wlan start hostednetwork
pause
```

今天更新驱动时发现了intel的my wifi便试了下, 可以实时显示都有哪些设备使用了.但是有个问题就是其他设备一直不能正常获取到ip地址.

打开my wifi创建的无线网络连接属性, 尝试着将除了internet 协议版本4 之外的其他项全取消了就好了,不知道什么原因导致的.
