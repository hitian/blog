---
aliases:
- /notes/2012/12/30/brew-install-httpd/
categories: notes
date: "2012-12-30T00:00:00Z"
tags:
- brew
- macos
title: brew install httpd 发生错误
---
今天安装subversion的过程中把httpd卸掉了， 结果重新安装的时候发生错误了

```bash
tian-mac:~ tian$ brew install httpd
==> Downloading http://www.apache.org/dist/httpd/httpd-2.2.22.tar.bz2
Already downloaded: /Library/Caches/Homebrew/httpd-2.2.22.tar.bz2
==> ./configure --prefix=/usr/local/Cellar/httpd/2.2.22 --mandir=/usr/local/Cellar/httpd/2.2.22/share/man --enable-layout=GNU --enable-mods-shared=all --with-ssl=/usr --wit
checking whether to enable mod_substitute... shared (all)
checking whether to enable mod_charset_lite... no
checking whether to enable mod_deflate... checking dependencies
checking for zlib location... not found
checking whether to enable mod_deflate... configure: error: mod_deflate has been requested but can not be built due to prerequisite failures
 
READ THIS: https://github.com/mxcl/homebrew/wiki/troubleshooting
 
These open issues may also help:
 
https://github.com/mxcl/homebrew/issues/14884
```

看了一下 zlib 已经安装了， 所以编辑一下安装的脚本加上zlib的路径

`brew edit httpd`

加上一行  `--with-z=/usr/local/opt/zlib/`

```bash

def install
   system "./configure", "--disable-debug",
                         "--disable-dependency-tracking",
                         "--prefix=#{prefix}",
                         "--mandir=#{man}",
                         "--enable-layout=GNU",
                         "--enable-mods-shared=all",
                         "--with-ssl=/usr",
                         "--with-z=/usr/local/opt/zlib/",
                         "--with-mpm=prefork",
                         "--disable-unique-id",
                         "--enable-ssl",
                         "--enable-dav",
                         "--enable-cache",
                         "--enable-proxy",
                         "--enable-logio",
                         "--enable-deflate",
                         "--with-included-apr",
                         "--enable-cgi",
                         "--enable-cgid",
                         "--enable-suexec",
                         "--enable-rewrite"
   system "make"
   system "make install"
 end

```

重新安装一下就好了。

