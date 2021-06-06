---
aliases:
- /other/2012/11/25/macos-install-dnsmasq/
categories: other
date: "2012-11-25T00:00:00Z"
tags:
- macos
- dnsmasq
title: mac下安装dnsmasq笔记
---

安装

`brew update && brew install dnsmasq`

 

配置

#从模板创建配置文件

`cp /usr/local/Cellar/dnsmasq/2.63/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf`

#开机自启动

`sudo cp /usr/local/Cellar/dnsmasq/2.63/homebrew.mxcl.dnsmasq.plist /Library/LaunchDaemons/`

`sudo launchctl load -w /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist `

 

编辑   `/usr/local/etc/dnsmasq.conf`

修改 `resolv-file=/etc/resolv.dnsmasq.conf`

如果只想本机使用 这里也修改一下

`listen-address=127.0.0.1`

 

修改maxos的配置使用本地的dnsmasq

编辑 `/etc/resolv.conf`

修改  `nameserver 127.0.0.1`

 

参考原始页面 [http://blog.philippklaus.de/2012/02/install-dnsmasq-locally-on-mac-os-x-via-homebrew/](http://blog.philippklaus.de/2012/02/install-dnsmasq-locally-on-mac-os-x-via-homebrew/)

 

 
