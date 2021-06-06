---
aliases:
- /other/2011/11/27/thinkpad-ubuntu-disable-touchpad/
categories: other
date: "2011-11-27T00:00:00Z"
tags:
- thinkpad
- ubuntu
title: thinkpad ubuntu 11.10 暂时禁用触控板
---
打字的时候经常碰到触控板很郁闷, 在使用usb鼠标的时候可以暂时禁用触控板和小红点

`sudo modprobe -r psmouse`


重新启用

`sudo modprobe -i psmouse`


