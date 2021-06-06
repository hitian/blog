---
aliases:
- /notes/2018/07/12/vmware-esxi-upgrade-ssl-certificate/
categories: notes
date: "2018-07-12T00:00:00Z"
tags:
- vmware
- esxi
- ssl
title: 更新vmware esxi ssl证书
---

esxi 的证书管理页面貌似有bug一直无法更新证书，这里直接ssh登陆服务器替换

开启SSH 

![vmware-exsi-enable-sshd](/assets/images/201807/vmware-exsi-enable-sshd.png)

Let's Encrypt 签发的证书目录如下

```bash

hitian :: ~ » ls -al .acme.sh/hitian.me/
total 36
drwxr-xr-x 2 tian tian 4096 Mar 20 14:59 .
drwx------ 8 tian tian 4096 May 27 15:41 ..
-rw-r--r-- 1 tian tian 1648 May 19 00:26 ca.cer
-rw-r--r-- 1 tian tian 3803 May 19 00:26 fullchain.cer
-rw-r--r-- 1 tian tian 2155 May 19 00:26 hitian.me.cer
-rw-r--r-- 1 tian tian  529 May 19 00:26 hitian.me.conf
-rw-r--r-- 1 tian tian  980 May 19 00:24 hitian.me.csr
-rw-r--r-- 1 tian tian  220 May 19 00:24 hitian.me.csr.conf
-rw-r--r-- 1 tian tian 1675 Mar 20 14:55 hitian.me.key

```

exsi 的配置目录如下

```bash

[root@localhost:/etc/vmware/ssl] ls -al
total 36
drwxr-xr-x    1 root     root           512 Jun  3 18:34 .
-r--r--r-T    1 root     root             0 Apr  3 21:24 .#castore.pem
-r-------T    1 root     root             0 Apr  3 21:24 .#rui.bak
-r--r--r-T    1 root     root             0 Apr  3 21:24 .#rui.crt
-r-------T    1 root     root             0 Apr  3 21:24 .#rui.key
drwxr-xr-x    1 root     root           512 Jul 12 00:28 ..
-rw-r--r--    1 root     root          1050 Jun  3 18:34 castore.pem
-rw-r--r--    1 root     root          3859 Jun  3 18:34 iofiltervp.pem
-r--r--r--    1 root     root           229 Apr  3 21:24 openssl.cnf
-r--------    1 root     root          5231 Jun  3 18:34 rui.bak
-rw-r--r--    1 root     root          3803 Jul 12 00:26 rui.crt
-rw-------    1 root     root          1675 Jul 12 00:27 rui.key
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_castore.pem
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_castore_old.pem
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_client.crt
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_client.key
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_client_old.crt
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsan_kms_client_old.key
-rw-r--r-T    1 root     root             0 Apr  3 21:50 vsanvp_castore.pem

```

* 使用 `fullchain.cer` 覆盖 `rui.crt`
* 使用 `hitian.me.key` 覆盖 `rui.key`

**覆盖前先备份一下，防止发生意外**

接下来需要重启UI让配置生效

```bash

/etc/init.d/hostd restart
/etc/init.d/vpxa restart

```