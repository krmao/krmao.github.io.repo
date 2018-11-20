---
title: 安装_CENTOS_系统新装
date: 2018-11-19 18:29:49
categories: [技术]
tags: [安装,CENTOS]
---

### 1 升级
```
yum upgrade -y
```

##### [关于 upgrade 与 update 参考大神见解](https://segmentfault.com/q/1010000011264410)
```
其实我更推荐用yum upgrade取代yum update，yum update只更新系统中已有的软件包，不会更新内核软件包(kernel-这个包)，yum upgrade是更彻底的update，会分析包的废弃关系，可以跨小版本升级（比如从centos 7.1升级到centos 7.4），除了做了yum update完全相同的事之外，还会更新kernel-的包，也会卸载掉已经废弃的包。

新部署系统需要yum update/upgrade是因为yum不会给你解决依赖冲突(但是apt会)。

举个例子，你的系统中已经安装了kernel-2.6.32.500，但是你要安装的某个软件包依赖于kernel-2.6.32.600，此时yum会报错退出，告诉你依赖不满足，并不会升级kernel包(只是举个例子而已，实际上几乎没有软件包直接依赖于kernel包)，所以你只能yum update/upgrade一次，把系统中所有的软件包全部更新，这样满足新部署的软件包的依赖。

在debian/ubuntu的系统中，apt会对这种情况自动处理，会自动升级依赖的软件包。

换句话来说，对于新部署的服务器，也是推荐upgrade全部的软件包，已获得最新的安全补丁。即使对于已经上线的服务器，也是推荐定期打安全漏洞补丁，减少漏洞带来的侵害。
```

### 2 不能 **ping通*** (解析)域名,但是能 **ping通** ip [参考链接](https://newbug.top/centos%E4%B8%8D%E8%83%BD%E8%A7%A3%E6%9E%90%E5%9F%9F%E5%90%8D/)

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0

# 在文件的最后加上dns的设置比如：DNS1=8.8.8.8
# 114.114.114.114和8.8.8.8，这两个DNS都可以，不用担心因ISP运营商导致的DNS劫持等问题，而且都是免费提供给用户使用的。
# 114.114.114.114是国内移动、电信和联通通用的DNS，如果你的服务器位于海外，建议使用谷歌的DNS
# 8.8.8.8是谷歌提供的DNS，该地址是全球通用的，如果你的服务器位于大陆，请不要使用。

DNS1=8.8.8.8

# 重启网卡
sudo service network restart
```
