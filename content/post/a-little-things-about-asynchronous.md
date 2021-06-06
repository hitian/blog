---
aliases:
- /js/2012/02/09/a-little-things-about-asynchronous/
categories: js
date: "2012-02-09T00:00:00Z"
tags:
- js
title: 关于异步的一点点其他东西
---
当一个网页包含大量的内容, 而只有其中的一小部分需要是动态内容(显示用户名, 积分等)时, 通常整个页面采用html静态页面, 并使用js来加载和改变动态的部分.

但是像这样, 如果js加载速度很慢的话, 会导致之下的页面停止渲染...

```html
<html>
<head>
<title>demo6-1</title>
</head>
<body>
<p>content...</p>
<script type="text/javascript" src="get_something_api.php"></script>
<p>content...</p>
</body>
</html>
```

于是我们将js写在页面下面, 等页面渲染的差不多了再加载它

```html
<html>
    <head>
        <title>demo6-1</title> 
    </head>
    <body>
        <p>content...</p>
        <p>content...</p>
        <a href="demo6.html">demo6.html</a> | 
        <a href="demo6-2.html">demo6-2.html</a> | 
        <a href="demo6-3.html">demo6-3.html</a>
        <img src="27218b6e.jpg" width="1920" height="1200" border="0" alt="">
        <script type="text/javascript" src="get_something_api.php"></script>
    </body>
</html>
```

看起来是一个比较好的解决方案, 但是新的问题出现了.

当由于网络或者程序执行效率的问题js的速度很慢的时候, 等待js加载过程中, 页面是可以正常操作的, 但是一旦操作有同一服务器上不同资源的请求时, 这个请求就完全卡住了, 例如点击页面上的链接.  同一服务器下资源并不能很好的多线程同时请求.

本地用sleep模拟一个需要长时间才能完成的响应.

```php
<?php
    //do something and need some times
    header('Content-Type:application/x-javascript');
    sleep(5);
    echo "alert('results');";
?>
```

在对get_something_api.php 的请求过程中, 点击页面内同域名下的链接, 而跳转在js加载完成后才会执行. 一般输出js可能不会执行太久, 但是当使用长连接进行一些页面信息的实时推送时, 这个问题会很大.

解决方案就是将get_something_api.php 放到不同的域名下去, 即使两个域名指的是同一个虚拟主机, 也可以让资源并行的加载.

