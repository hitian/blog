---
aliases:
- /php/2011/08/22/php-regex-length-limit/
categories:
- php
category_backup: php
date: "2011-08-22T00:00:00Z"
tags:
- php
- regex
title: php注意正则匹配的长度限制
---
今天要使用正则来批量替换一个抓取页面中的图片地址
<!--more-->

```php
<?php
$nrjl = preg_replace_callback('#<img.*src="([^"]+)"#isU', "tianreplace", $nrjl);  
function tianreplace($matches){  
    if(substr($matches[1],0,7)=="http://"){   
        return '<img src="' . $matches[1] .'"';
    }else{  
        global $this_domain;  
        return '<img src="http://'.$this_domain.'/' . $matches[1] .'"';   
    }   
}
?>
```

```ini
ini_set('pcre.backtrack_limit', 10000000);
```


就正常了.
