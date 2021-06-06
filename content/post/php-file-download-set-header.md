---
aliases:
- /php/2011/12/20/php-file-download-set-header/
categories: php
date: "2011-12-20T00:00:00Z"
tags:
- php
- download
- header
title: php 文件下载 header 设置
---
读取文件, 强制浏览器进行下载

<!--more-->

`Content-Encoding: none`

没有的话 迅雷7会自动加上扩展名.gzip

Content-type

我要下载的是.exe文件, 所以设置为`application/octet-stream`

```php

<?php
ob_end_clean();
header("HTTP/1.1 200 OK"); 
header('Cache-control: max-age=31536000');
header('Content-Encoding: none');
header("Content-type:application/octet-stream");
header('Content-Disposition: attachment; filename=' . $file_name);
header('Content-Length: ' . filesize($file_path));
// 打开文件(二进制只读模式)
$fp = fopen($file_path, 'rb'); 
// 输出文件
fpassthru($fp); 
// 关闭文件
fclose($fp);

```

