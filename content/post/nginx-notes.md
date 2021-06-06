---
aliases:
- /notes/2013/04/04/nginx-notes/
categories: notes
date: "2013-04-04T00:00:00Z"
tags:
- nginx
- linux
title: nginx 使用笔记
---
添加phpmyadmin alias

```bash

location /phpmyadmin {
    root /usr/share/;
    index index.php index.html index.htm;
        location ~ ^/phpmyadmin/(.+\.php)$ {
                try_files $uri =404;
                root /usr/share/;
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include /etc/nginx/fastcgi_params;
        }
        location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
                root /usr/share/;
        }
    }

```

静态文件缓存， 并且不记录日志

```bash

location ~ \.(jpg|jpeg|gif|css|png|js|ico|xml)$ {
    access_log  off;
    expires     30d;
}

```
