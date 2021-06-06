---
aliases:
- /notes/2014/03/13/cocos2d-x-note/
categories: notes
date: "2014-03-13T00:00:00Z"
tags:
- game
- cocos2d-x
title: cocos2d-x 入门笔记
---
首先cocos2d-x开发游戏确实还是比较简单的. 因为可以使用js作为开发语言, 本来就会js的人入门会更快, 不过市面上的书和网上的各种教程基本都是用c++的, 所以学习过程中还是走了不少弯路. 

目前使用js作为开发语言的话, 可以在iOS, Android, Windows 上运行, 开发环境可以是MacOS+Xcode 或者Windows+VS, 不过Windows上那是非常的慢. 所以下面的都是在Mac下的. 

开始了...

官网: [http://www.cocos2d-x.org/](http://www.cocos2d-x.org/) 
`如果很慢请尝试使用代理`

##api文档##

[官方api文档地址>>](http://www.cocos2d-x.org/wiki/Reference)

对api的使用建议: 如果是使用js开发手机native的游戏的话, 可以使用Cocos2d-html5 的api文档配合Cocos2d-X的文档看, 因为Cocos2d-x的文档主要以c++的为主, 看js的话用起来非常的不方便, 所以我平时都是直接看html5的, 有问题的话再查一下c++的.

##创建项目##

这是第一个坑, 目前的稳定版本是2.2.2. 大部分书还是2.2的版本, 书里介绍的导入xcode模板的脚本现在已经没有了, 现在需要执行脚本直接生成工程.

脚本地址 `cocos2d-x-2.2.2/tools/project-creator/create_project.py`

建议使用 `python2.7` 来执行, 最新版的python貌似有问题

直接执行会有参数的提示, 把参数都补上再执行一遍项目就应该创建好了.

生成的项目会放在 `cocos2d-x-2.2.2/projects/` 目录下. iOS的xcode工程在 proj.ios 下面. 要运行非常简单, 直接在Xcode里点运行就可以直接在模拟器上运行自带的HelloWorld程序了. android的运行可能要稍微麻烦一点, 可以自行Google.

##学习##

因为这方面的资料大部分都是c++的, 所以学习也基本是从看c++的书开始, 其实也不用会c++, 看书主要是为了大概知道cocos2d-x是如何工作的, 都有什么样的方法或者工具可以使用, 接下来就是看源代码包里提供的js的示例项目, 看一下js是怎么实现各种功能的. 

官方给了5个js的示例项目, 在目录: `cocos2d-x-2.2.2/samples/Javascript/` 下, 注意js的源代码放在公用的 `cocos2d-x-2.2.2/samples/Javascript/Shared/games` 目录下. 认真研究一下这些代码, 在配合api文档基本上就可以开始尝试着开发自己的游戏了. 入门还是比较简单的.



