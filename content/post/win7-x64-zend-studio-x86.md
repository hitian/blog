---
aliases:
- /php/2011/11/05/win7-x64-zend-studio-x86/
categories:
- php
category_backup: php
date: "2011-11-05T00:00:00Z"
tags:
- php
- zend studio
title: win7 x64 运行x86的zend studio 9.0
---

x64的win8 安装了x64 的jdk  再运行zendstudio时就会出错

解决:

1. 再安装一下x86 的jre

2. 打开 C:\Program Files (x86)\Zend\Zend Studio 9.0.0\ZendStudio.ini

添加 -vm

C:\Program Files (x86)\Java\jre6\bin\javaw.exe


```
-name
Zend Studio
-vm
C:\Program Files (x86)\Java\jre6\bin\javaw.exe
-vmargs
```

