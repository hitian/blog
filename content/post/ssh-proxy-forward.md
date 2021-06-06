---
aliases:
- /notes/2019/01/05/ssh-proxy-forward/
categories: notes
date: "2019-01-05T00:00:00Z"
tags:
- ssh
- dev
- proxy
- forward
title: ssh 代理和转发那些事
---

# 配置ssh客户端的行为

配置文件路径 `~/.ssh/config`

针对目标主机的配置块示例

``` bash
Host deploy-gw
  HostName [__target_hostname_or_ip__]
  User game
  IdentityFile ~/.ssh/deploy_private_key
  ProxyCommand [__connect_command__]

```

## 配置解释

### 除了Host之外， 其它的都是可选的

* `Host deploy-gw`

定义使用时的名字，之后使用 `ssh deploy-gw` 登录目标，这个只是别名，当然这里也可以直接写hostname 或者IP地址; 当使用 Hostname 时可以进行匹配， 例如 `Host *.hitian.info` 可以匹配到 `a.hitian.info`.

* `HostName [__target_hostname_or_ip__]`

目标的主机名或者IP地址

* `User tian`

用户名

* `IdentityFile ~/.ssh/deploy_private_key`

指定使用的private key路径。 替代ssh的 `-i` 参数一样效果

* `ProxyCommand ssh -q [__local_gw__] nc [__target_addr__] 22`

连接目标主机时使用的命令

# ssh via [http or socks proxy]

使用场景: 由于不可抗拒的因素无法直接ssh 到远程主机，或者链接过程中会被异常阻断时

这里我们需要一个 connect 命令，需要自行安装一下。

``` text
Provides SOCKS and HTTPS proxy support to SSH
https://bitbucket.org/gotoh/connect/wiki/Home
```

macOS 可以利用brew 直接安装, 其它平台可以按说明执行编译安装

``` bash
brew install connect
```

要让ssh使用proxy连接到目标主机，在config 中添加ProxyCommand配置

使用 http 代理

`ProxyCommand connect -H http_proxy_addr:port %h %p`

使用 socks 代理

`ProxyCommand connect -S socks_proxy_addr:port %h %p`

详细的connect使用说明可以参照官方wiki

# ssh via ssh

使用场景：

例如需要通过某台ssh的网关才能链接到内网的其它ssh服务器

当然可以通过先ssh到网关，再ssh到目标主机的方式简单实现，但是这要求在网关机器上放一份目标主机的私钥。

如果不想将私钥给网关机器。可以采用以下方式

使用 nc 命令实现转发， nc在macOS和主流Linux上已经内置

---

在config 中添加ProxyCommand配置

`ProxyCommand ssh -q gateway_host nc target_host 22`

这里 gateway_host 为网关， nc 后为目标的主机和sshd的端口号， 首先要保证可以正常ssh 到gateway_host。

这种方式可以直接实现更长的转发链

例如要直接在local登录target 只需要在本地分别为a, b, target 配置ProxyCommand就可以了，妈妈再也不需要担心我要一层一层ssh到远程机器了:)

```text
local -> gateway -> a -> b -> target
```

# socks proxy over ssh

使用场景：需要访问目标主机的localhost，或者目标主机内网的资源时， 如果客户端无法使用socks链接需要的资源，请参考下一种方式

在本地监听1089端口。将请求通过ssh隧道转发到target_host

```bash
ssh -D 1089 target_host
```

# port forward via ssh

使用场景： 直接在本地请求目标主机内网的服务时

举个例子

gateway 服务器的内网有一台mysql服务器运行在 `10.0.0.3:3306`， 本地的mysql客户端无法直接链接。 我们可以采用ssh来映射端口到本地

```bash
ssh -L localhost:3306:10.0.0.3:3306 target_host
```

L参数的格式  `本地地址`:`本地监听端口`:`远程地址`:`要映射的远程端口`

上面的命令可以直接将远程mysql服务器的3306端口映射到本地的3306端口， 在本地可以直接使用`mysql -h localhost`连接。

当然也可以使用0.0.0.0，将本地的端口开放给本地的其它主机使用

```bash
ssh -L 0.0.0.0:3306:10.0.0.3:3306 target_host
```

# 不稳定的网络中使用ssh

由于ssh使用的TCP链接，在不稳定的网络中使用ssh会很容易断开，推荐使用 mosh [https://mosh.org/](https://mosh.org/)

mosh 会使用 Protocol Buffers + UDP 的方式传输ssh数据，包含数据的压缩和断线重连，原有的ssh配置和权限都不需要额外的变更

注意：mosh客户端在启动的时候会ssh到远程的主机，启动一个mosh-server 并默认监听在一个60000-61000之间的UDP端口。而且每开启一个新的客户端， 都会在服务器上开一个新的udp端口，需要防火墙放心对应的UDP端口段.

iOS上的客户端推荐 [https://github.com/blinksh/blink](https://github.com/blinksh/blink)

THE END