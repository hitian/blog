---
aliases:
- /develope/2014/03/15/xcode5.1-cocos2d-x-error/
categories:
- develope
category_backup: develope
date: "2014-03-15T00:00:00Z"
description: cocos2d-x2.2.2在xcode5.1下编译问题临时解决
tags:
- xcode
- cocos2d-x
title: cocos2d-x2.2.2在xcode5.1下编译问题临时解决
---

iOS7.1发布之后Xcode也更新到5.1了, 然后之前的程序就跑不起来了. 网上找的解决.


如果出现

`Unknown register name 'q0' in asm`

请参照 [http://stackoverflow.com/questions/21510187/unknown-register-name-q0-in-asm](http://stackoverflow.com/questions/21510187/unknown-register-name-q0-in-asm)

将 `neon_matrix_impl.c` 中的 `#if defined(ARM_NEON)`  修改为  `#if defined(_ARM_ARCH_7)`


另外出现的

`EAGLView.mm:408:18: Cast from pointer to smaller type 'int' loses information`

可以参照 [http://www.cocoachina.com/bbs/read.php?tid=193991#902811](http://www.cocoachina.com/bbs/read.php?tid=193991#902811) 5楼给出的方案修改  Targets—>Build Settings—>Architectures 的配置, 将64bit的给去掉.




PS. 突然觉得作为刚开始入门的新手, 还是得谨慎升级. 特别是解释型脚本语言用习惯, 处理编译问题真头大.





