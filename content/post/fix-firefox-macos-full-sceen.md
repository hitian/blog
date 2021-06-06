---
aliases:
- /notes/2020/08/01/fix-firefox-macos-full-sceen/
categories: notes
date: "2020-08-01T00:00:00Z"
tags:
- macos
- firefox
title: 修复macOS上的Firefox在全屏时会隐藏所有屏幕菜单栏的问题
---

针对Firefox for macOS 79.0 (64-bit)

如果macOS有多块屏幕， 在默认配置下 firefox 全屏时会隐藏所有屏幕的菜单栏，需要手动改配置修复。

1. 地址栏输入并打开 `about:config`
2. 忽略风险提示并搜索 `full-screen-api`
3. 双击 `full-screen-api.macos-native-full-screen` 将配置更改为 `true`

![firefox full screen api config](/assets/images/202008/2020-08-01-fix-firefox-macos-full-sceen.markdown.png)
