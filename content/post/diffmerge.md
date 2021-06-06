---
aliases:
- /other/2012/12/01/diffmerge/
categories: other
date: "2012-12-01T00:00:00Z"
tags:
- DiffMerge
- finder
- macos
title: 为DiffMerge创建finder右键菜单
---

习惯了win下直接选中文件右键对比， 在mac下找了好久，要么贵的要死，要么没有，所以决定自己创建一个。

<!--more-->

1. 打开系统自带的 Automator

![Snip20121201 3](/assets/images/2012/12/Snip20121201_3.png "Snip20121201_3.png")

2. 创建一个服务

![Snip20121201 5](/assets/images/2012/12/Snip20121201_5.png "Snip20121201_5.png")

3. 右上  “服务”收到选定的  改为 文件货文件夹  ， 位于改为Finder.app

4. 左边选 使用工具  双击下面的 运行Shell 脚本

5. 右边 传送输入 改为 作为自变量，  下面输入  `/Applications/DiffMerge.app/Contents/MacOS/DiffMerge "$1" "$2"`

    参考自己的DiffMerge的位置

好了， 现在取个名字并保存

![Snip20121201 7](/assets/images/2012/12/Snip20121201_7.png "Snip20121201_7.png")

选中要对比的文件， 右键 -》服务   就可以看到了。

