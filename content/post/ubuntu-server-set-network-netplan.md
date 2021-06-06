---
aliases:
- /notes/2018/07/10/ubuntu-server-set-network-netplan/
categories: notes
date: "2018-07-10T00:00:00Z"
tags:
- ubuntu
- network
- netplan
title: 使用netplan管理ubuntu 的网络配置
---

最新的ubuntu已经使用`netplan`来管理网络配置了

列出当前的配置文件`ls -al /etc/netplan/`， 注意配置文件为yaml格式， 如果当前没有配置， 可以创建一个例如`/etc/netplan/01-netcfg.yaml`

## 例子

### 配置为使用DHCP

```yaml

network:
 version: 2
 renderer: networkd
 ethernets:
   ens33:
     dhcp4: yes
     dhcp6: yes

```


### 配置为静态ip

```yaml

network:
 version: 2
 renderer: networkd
 ethernets:
   ens33:
     dhcp4: no
     dhcp6: no
     addresses: [192.168.1.2/24]
     gateway4: 192.168.1.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]

```

配置文件修改好之后使用`sudo netplan apply`应用配置， 可以加debug参数来查看具体的过程`sudo netplan --debug apply`