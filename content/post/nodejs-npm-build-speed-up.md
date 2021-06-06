---
aliases:
- /notes/2015/04/06/nodejs-npm-build-speed-up/
categories: notes
date: "2015-04-06T00:00:00Z"
tags:
- nodejs
- npm
- node-gyp
- 加速
title: nodejs 相关的墙内加速
---

npm的加速比较简单, 直接使用淘宝的 [http://npm.taobao.org/](http://npm.taobao.org/) 就可以了.

建议直接安装 cnpm, 方便一点

`npm install -g cnpm --registry=https://registry.npm.taobao.org`

使用的时候直接用`cnpm`替代`npm`就可以了. 例如:

`cnpm install mongodb`

也可以在使用npm 的时候单独指定

`npm install xxx  --registry=https://registry.npm.taobao.org`

这样就快多了.


另外有时候 node-gyp rebuild  过程中下载相应node版本也很慢.

看提示去手动下载需要的node版本.

例如 node-v0.12.2.tar.gz

```bash
tar zcvf node-v0.12.2.tar.gz
mv ./node-v0.12.2 ~/.node-gyp/0.12.2
touch ~/.node-gyp/0.12.2/installVersion
echo "9" >~/.node-gyp/0.12.2/installVersion
```
