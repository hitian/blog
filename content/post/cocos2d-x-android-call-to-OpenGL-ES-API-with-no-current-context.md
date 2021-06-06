---
aliases:
- /cocos2d-x/2014/09/05/cocos2d-x-android-call-to-OpenGL-ES-API-with-no-current-context/
categories:
- cocos2d-x
category_backup: cocos2d-x
date: "2014-09-05T00:00:00Z"
description: cocos2d-x 在Android 上出现 call to OpenGL ES API with no current context
  的解决
tags:
- cocos2d-x
- android
- js
- opengl
title: cocos2d-x 出现 call to OpenGL ES API with no current context
---

环境 cocos2d-x 2.2.2 + js

游戏的某些功能界面在Android出现了很奇怪的黑块, LOG记录到

`E/libEGL  (30687): call to OpenGL ES API with no current context (logged once per thread)`

google一下, 大概的原因可能是在非UI线程上操作场景上的图片资源导致的. 但是网上的情况绝大多数情况出现在 java 操作 c++ 层时出现的, 而我们是直接出现在js的回调方法中.

回调是由某sdk执行的, 通过 `java -> c++ -> js` 一层层回来的.

也就是说最终js的回调有可能是执行在非ui线程里的, 所以在这里更新界面有可能会出现问题.

囧...

推测

js无法决定自己运行在什么线程里, 但是可以控制界面更新在UI线程里操作.

我们的流程是这样的.

`sdk -> someService -> controller -> view`

那其实我们可以打断someService 和 controller 之间的直接调用关系. 分成两步来执行.

`sdk -> someService -> [write status mark & store params]`

写完状态就可以直接返回了. 剩下的事情由游戏的循环来做. 游戏的循环本来就是负责界面更新的, 所以肯定不会有问题.

`gameloop -> [check status mark & do it]`

可以采用在游戏循环里去重复检测需要执行的方法. 当然如果只是一次调用最省资源的还是直接用cocos2d-x 提供的scheduler相关的方法.

android在游戏开发方面的表现确实很奇葩, 加上某些不开源的SDK, 会出现什么问题就很难预测了, 为了以防万一, 甚至可以在someService -> controller 之前加一个的操作队列来彻底隔离有可能在错误的线程执行的问题.


