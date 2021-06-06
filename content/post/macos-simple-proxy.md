---
aliases:
- /other/2012/10/01/macos-simple-proxy/
categories: other
date: "2012-10-01T00:00:00Z"
tags:
- macos
- proxy
title: mac上最简单的代理方式
---

在mac上使用ssh代理其实很方便

前提是有一个ssh账号

命令

`$ ssh -D 1089 tian@domain.com`

会打开端口1089 监听

查看端口可以发现

```
Active Internet connections (including servers)
Proto Recv-Q Send-Q Local Address Foreign Address (state) 
tcp6 0 0 ::1.1089 *.* LISTEN 
tcp4 0 0 127.0.0.1.1089 *.* LISTEN
```

这样连接就建立好了， 然后配置一下

进入网络偏好设置， 选自己正在用的网络连接 -》点高级-》选择代理标签

![Snip20121001 3](/assets/images/2012/10/Snip20121001_3.png "Snip20121001_3.png")

使用socks代理， 填上刚才的端口号

确实非常方便。
