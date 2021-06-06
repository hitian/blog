---
aliases:
- /other/2012/09/10/blog-comments/
categories: other
date: "2012-09-10T00:00:00Z"
tags:
- blog
title: blog评论反垃圾
---

近几天博客的垃圾评论越来越多， 一天甚至好几十个。 迫不得已装个插件来防垃圾。

由于空间禁用了php 的fsockopen 函数导致Akismet 无法连接到服务器。

测试一下发现pfsockopen 没有被禁用， 进入akismet的目录批量替换所有fsockopen为pfsockopen。

![QQ20120910 1](/assets/images/2012/09/QQ20120910-1.png "QQ20120910-1.png")

然后就检测正常了

具体的功能好不好使得观察一段时间再看了。
