---
aliases:
- /other/2012/11/25/openwrt-dns-configuration/
categories: other
date: "2012-11-25T00:00:00Z"
tags:
- openwrt
- dns
title: openwrt dns配置
---

想使用v2ex的dns来加速app store， 又不想因为dns的原因导致某些网站的区域性cdn加速失效

其实用dnsmasq很容易解决这个问题

给路由器刷个openwrt， 然后设置就简单多了

打开  网络   =》   DHCP/DNS   页面

设置一下里面的DNS转发， 把v2ex的两个ip加进去, 将苹果的二级域名使用v2ex进行解析， 而其他域名还是通过本地isp的dns解析。

`/.apple.com/199.91.73.222`

`/.apple.com/178.79.131.110`

![Snip20121125 4](/assets/images/2012/11/Snip20121125_4.png "Snip20121125_4.png")

保存， 大功告成

 
