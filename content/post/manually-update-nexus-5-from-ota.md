---
aliases:
- /notes/2015/03/20/manually-update-nexus-5-from-ota/
categories: notes
date: "2015-03-20T00:00:00Z"
tags:
- nexus
- ota
- upgrade
title: 手动从OTA包升级nexus 5
---

Android 5.1 发布已经有一段时间了, nexus5的ROM出来也有一段时间了, 但是OTA一直不推送, 挺着急的. google了一下, 决定自己升级一下.

####先去下载OTA的升级包

nexus5 从5.0.1 到 5.1 的包: [百度网盘](http://pan.baidu.com/s/1jGIe4KY)

其他的这里下载: [link](http://www.androidguys.com/2015/03/17/all-the-android-5-1-ota-download-links/)

####其他的准备

* 没有adb的先去装一下
* 将手机的`开发者模式`打开(连按`关于手机`中的`版本号`)
 

####开始了

1. 先关机
2. 按住 `音量下` 和 `电源键` 启动进入`FASTBOOT MODE`
3. 按`音量上下` 选择 `Recovery mode`, 按`电源键`进入
4. 自动重启后会显示一个感叹号的机器人, 下面显示`无命令`, 按住`音量上`和`电源键`进入recovery 的菜单.
5. 选择`apply update from ADB` 并确定 
6. 连接手机到电脑, 命令行进入下载的包目录, 执行`adb sideload update.zip`
7. 等待完成, 大概需要6分钟左右, 然后选择 `reboot system now`
8. 完成了.

升级完成了:

[![image](/assets/images/201503/nexus5_ota_1.jpg "升级画面")](/assets/images/201503/nexus5_ota_1.jpg)

重新启动过程中会升级已有的App, 这个只能耐心等待了..


参考: [http://www.androidbeat.com/2014/12/manually-sideload-android-lollipop-update-nexus-5/](http://www.androidbeat.com/2014/12/manually-sideload-android-lollipop-update-nexus-5/)