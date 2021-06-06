---
aliases:
- /notes/2013/04/12/redis-notes/
categories: notes
date: "2013-04-12T00:00:00Z"
tags:
- php
- redis
title: redis使用笔记
---
根据 [http://redis.io](http://redis.io) 的说明， redis支持 string， list， set， sorted set 和 hash 这五种数据类型， php中可能经常需要直接把array 直接存起来。

仔细看了下 [https://github.com/nicolasff/phpredis](https://github.com/nicolasff/phpredis)  上的说明， 其实有个方法setOption 可以设置客户端的一些属性

```php

<?php

$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_PHP);    
// use built-in serialize/unserialize

```

这样就可以使用php内置的serialize/unserialize 方法对数据进行处理

```php

<?php
/**
 * redis test
 * @author jia.tian@me.com
 */
 
$redis = new Redis();
$redis->connect('127.0.0.1', '6379');
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_PHP);
 
$redis->set('tian', array('name' => 'tian', 'passwd' => '123456'));
var_dump($redis->get('tian'));
 
$redis->set('tian', 'test');
var_dump($redis->get('tian'));

```

运行结果：

`array(2) { ["name"]=> string(4) "tian" ["passwd"]=> string(6) "123456" } string(4) "test"`

可以看到将array存入和取出时会自动进行了处理这是直接在github上下载的zip包在ubuntu上安装的，可以看到版本号是2.2.2

[![Snip20130412_7](/assets/images/2013/04/Snip20130412_7.png)](/assets/images/2013/04/Snip20130412_7.png)

因为公司电脑是win环境， 找了个win的redis扩展，但是在setOption这步有点麻烦， 只要一设置就白屏。 原因仍在测试中。。。

ps：项目终于要开始大量使用redis了， 能到这一天真不容易啊。
