---
aliases:
- /other/2011/11/22/restore-development-environment/
categories: other
date: "2011-11-22T00:00:00Z"
tags:
- dev
- php
- workspace
title: 重装系统后恢复开发环境
---

每次做系统都很愁,  因为有一大堆软件需要重新安装, 尽管尽量都用绿色的还是有一大推需要安装的, 尝试着列个表, 简化一下流程...
 
 
### 个人文件夹迁移 ###

将个人文件夹中的 我的视频 我的音乐 我的图片 移动到原来非系统盘的位置去 (一直很纳闷为什么这些需要大量占用磁盘空间的东西不一开始就放在非系统盘)
  
 
### 恢复apache和php环境 之前的都装在d盘了  ###
 
以管理员运行cmd 进入D:\lamp\apache\bin
 
`httpd.exe  -k install`   将apache安装为系统服务
 
`httpd.exe  -k start`      启动apache
 
mysql 一直使用的远程的, 跳过
 
将备份的host 文件覆盖 顺便调整一下修改的权限
 
运行环境搞定 
 
 
 
### 安装firefox 然后安装插件 ###
 
firebug; 
 
firecookie; 
 
web developer; 
 
dns flusher; 
 
advanced cookie manager;
 
再安装一下chrome  使用google帐号将之前的插件都同步回来
 
ok 调试环境也搞定
 
 
 
### 安装 jre ;  Dreamweaver ; mysql workbench; eclipse pdt; editplus ###
 
ok 开发环境搞定
