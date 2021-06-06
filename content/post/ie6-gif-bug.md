---
aliases:
- /js/2011/11/25/ie6-gif-bug/
categories: js
date: "2011-11-25T00:00:00Z"
tags:
- ie6
- js
- gif
title: 解决ie6 恶心的动态gif图片问题
---
将改变img标签的src的事件触发写在a标记的onclick里面 在ie6下会导致切换后新的图片无法加载.

今天又发现了另一个恶心的问题
<!--more-->

```html

<img src="/images/pic1.gif" />
<img src="/images/pic2.gif" style="display:none" />
<a href="javascript:;" onclick="chenge_pic();">切换图片</a>

```

像这样, 如果pic2.gif 是一个动态的图片, 当点击a标签切换图片到pic2.gid 时, 实际看到的pic2.gif 是不会动的.

直接将第一个标签的src 改成pic2.gif 图片也是不会动的.

和之前的解决方案一样, 将a标记换成 `<input type="button"  />` 就好了.恶心的ie6, 天天画圈圈诅咒它快点消失....
