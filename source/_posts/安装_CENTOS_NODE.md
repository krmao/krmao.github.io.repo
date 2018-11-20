---
title: 安装_CENTOS_NODE
date: 2018-11-19 18:29:49
categories: [技术]
tags: [安装,CENTOS]
---

# 1 下载地址 [https://nodejs.org/en/download/](https://nodejs.org/en/download/)
```
cd /opt/software/packages/
wget https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x64.tar.xz
```

# 2 安装
```
tar -xvf /opt/software/packages/node-v8.10.0-linux-x64.tar.xz -C /opt/software/
cd /opt/software/node-v8.10.0-linux-x64/bin/ && ls
./node -v
```
```
v8.10.0
```

# 3 增加软连接,使得 node/npm 可以全局使用
```
ln -s /opt/software/node-v8.10.0-linux-x64/bin/node /usr/local/bin/node
ln -s /opt/software/node-v8.10.0-linux-x64/bin/npm /usr/local/bin/npm
```
```
# 测试
cd /home && node -v && npm -v
```
```
v8.10.0
5.6.0
```
