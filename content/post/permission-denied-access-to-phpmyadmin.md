---
aliases:
- /linux/2011/12/13/permission-denied-access-to-phpmyadmin/
categories: linux
date: "2011-12-13T00:00:00Z"
tags:
- linux
- centos
title: 关于(13) phpmyadmin Permission denied
---

centos5.4 配置完apache+php+mysql 环境后出现了一个很棘手的问题

使用 Alias /phpmyadmin "/var/www/html/phpmyadmin/"

之后访问 http://localhost/phpmyadmin/index.php 一直出403

/var/www/html/phpmyadmin/  目录的owner 改为apache

并给了整个目录755的权限

在访问的时候仍然出现了403

解决方案:

`#chcon -R -h -t httpd_sys_content_t  /var/www/html/phpmyadmin/`

本来一直使用ubuntu server 的, 没有用过centos, 为了明天的考核不得不试了下centos , 结果就出现这样纠结的问题, 翻了好久的英文网页才找到解决办法, 感谢google...
