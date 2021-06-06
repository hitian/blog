---
aliases:
- /notes/2017/02/16/acme-sh-create-letsencrypt-certificates-with-dns-api/
categories: notes
date: "2017-02-16T00:00:00Z"
tags:
- ACME
- acme.sh
- dnspod
- Let's Encrypt
- certificate
title: 使用ACME Shell script和dnspod的api自动生成Let's Encrypt证书
---

### 安装 acme.sh 

```bash

curl https://get.acme.sh | sh

```

参考 [https://github.com/Neilpang/acme.sh](https://github.com/Neilpang/acme.sh)

安装完成之后可执行文件位于 `~/.acme.sh/acme.sh`

### 生成 dnspod 的API Token

地址 [https://www.dnspod.cn/console/user/security](https://www.dnspod.cn/console/user/security)

用户中心 > 安全设置 > API Token > 创建 API Token

注意，Token 只显示一次， 保存下来， 如果忘记了， 就只能删除然后重新生成了。

还需需要记录下 API Token 的 ID

### 开始签发证书

参考 [https://github.com/Neilpang/acme.sh/tree/master/dnsapi](https://github.com/Neilpang/acme.sh/tree/master/dnsapi)

首先设置环境变量

```bash

export DP_Id="API Token 的 ID"
export DP_Key="API Token"

```

然后可以开始签发了

```bash

acme.sh --issue --dns dns_dp -d example.com -d www.example.com

```

每个 `-d` 参数可以指定一个域名， 可以把需要用到的子域名也全部列出来

自动运行的流程大概是这样的

* 使用dnspod的api 自动生成所有的验证域名txt记录 _acme-challenge.example.com， 每个子域名也会有_acme-challenge.www.example.com
* 等待dns记录生效，自动脚本会sleep 120 秒
* 检查验证的dns记录， 没有问题的话签发证书保存到本地， 再次调用api 移除验证的域名
* 创建crontab 自动更新相关证书。

生成好的证书位于 `~/.acme.sh/www.example.com` 目录下, 至此证书就签发好了。

另外自动创建的 crontab 

`24 0 * * * "/home/tian/.acme.sh"/acme.sh --cron --home "/home/tian/.acme.sh" > /dev/null`

应该会每天运行一次， 在证书过期前自动更新。

