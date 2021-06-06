---
aliases:
- /ubuntu/2011/11/09/ubuntu-server-upgrade/
categories: ubuntu
date: "2011-11-09T00:00:00Z"
tags:
- ubuntu
- linux
- server
- update
title: ubuntu-server 版本升级
---

一直以来虚拟机里测试用的ubuntu server 都是在出新版本后直接重新安装的, 但是现在东西实在太多了, 懒得再重装了, 网上查了一下升级的过程作下记录:

升级之前重要的配置文件最好先备份一下, 对于测试服务器, 我选择直接跳过

修改一下/etc/apt/sources.list 找个快点的源, 不然会很郁闷的. 网易的就不错 mirrors.163.com

开始升级

`$sudo apt-get install update-manager-core`

显示已经安装了最新版本

接着

`$sudo do-release-upgrade`

因为我是ssh上去的, 系统会提示如果发生意外可以使用1022端口登录上去

一路 y

[![image](/assets/images/2011/11/image_thumb.png "image")](/assets/images/2011/11/image.png)

[![image](/assets/images/2011/11/image_thumb1.png "image")](/assets/images/2011/11/image1.png)

可以看到其实已经在get新版本的包了

接下来漫长的半个小时等待..zzzZZ

网速严重不给力啊

安装新的, 有一部分老的会被删除

[![image](/assets/images/2011/11/image_thumb2.png "image")](/assets/images/2011/11/image2.png)

接着自动重启

[![image](/assets/images/2011/11/image_thumb3.png "image")](/assets/images/2011/11/image3.png)

看来升级成功了

[![image](/assets/images/2011/11/image_thumb4.png "image")](/assets/images/2011/11/image4.png)

看来mysql没起来, 其他都是正常的

[![image](/assets/images/2011/11/image_thumb5.png "image")](/assets/images/2011/11/image5.png)


(完)...
