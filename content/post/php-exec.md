---
aliases:
- /php/2011/10/20/php-exec/
categories:
- php
category_backup: php
date: "2011-10-20T00:00:00Z"
tags:
- php
- server
- exec
title: php执行服务器脚本
---
php执行服务器上的脚本进行一些操作不是什么难事.

exec system passthru 这几个函数都可以调用外部的命令. 只要有权限就完全没有问题.

例如 

```exec('/home/tian/bin/trytodosomething -h');```

命令执行后php会挂起直到命令运行完毕.
有时候脚本运行的时间也许很长, 我们需要让php继续执行及时反馈给用户我们已经开始操作了.

```
$cmd = '/home/tian/bin/trytodosomething -h > /tmp/null &';
$result = system($cmd);
```

这样修改之后, php就会继续执行, 不会等待命令执行完这样也存在一个问题, 用户不断的刷新网页可以能造成一些麻烦, 使用文件锁就可以解决这个问题. 
