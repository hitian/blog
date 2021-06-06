---
aliases:
- /php/2012/04/29/sql-injection-risk/
categories: php
date: "2012-04-29T00:00:00Z"
tags:
- php
- sql
title: 注入风险提醒
---
前几天发现的数据库注入总结一下

发生问题的过滤:

```php
<?php
$type = ( in_array($_GET['t'],array(1,2,3)) )?$_GET['t']:0;

```

这个过滤实际不严格, 当传进来的变量以 1,2,3中的任意数字开头时过滤将失败

当用户 传 `t=1' or '1'='1` 时, $type获取到的变量将维持原样, 直接传到数据库进行查询时会有注入危险

总结和建议:所有由用户传入int 值 必须使用 `intval()` 进行过滤

字符串必须使用`mysql_real_escape_string()` 进行过滤

在读取 `$_GET $_REQUEST $_POST $_COOKIE` 时 必须进行过滤, 一般认为用户传入的数据都是不安全的.
