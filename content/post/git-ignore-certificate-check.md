---
aliases:
- /other/2013/06/23/git-ignore-certificate-check/
categories: other
date: "2013-06-23T00:00:00Z"
tags:
- git
title: 设置git忽略服务器证书校验错误
---
将自己的git服务器切换到https后clone时发生错误

```
error: SSL certificate problem, verify that the CA cert is OK. Details:
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed while accessing https://...
```

因为证书是自己颁发的无法通过CA的校验

`git config --global http.sslVerify false`

这样就可以忽略校验了.

话说gitlab版本从5.2升级到5.3还真顺利, 升级文档: 

[https://github.com/gitlabhq/gitlabhq/blob/master/doc/update/5.2-to-5.3.md](https://github.com/gitlabhq/gitlabhq/blob/master/doc/update/5.2-to-5.3.md "https://github.com/gitlabhq/gitlabhq/blob/master/doc/update/5.2-to-5.3.md")
