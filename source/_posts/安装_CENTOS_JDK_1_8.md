---
title: 安装_CENTOS_JDK_1_8
date: 2018-11-19 18:29:49
tags: [安装,CENTOS]
---

# 1 下载 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
```
cd /opt/software/packages/
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u162-b12/0da788060d494f5095bf8624735fa2f1/jdk-8u162-linux-x64.tar.gz
```

# 2 安装
```
# 卸载 centos 默认安装的 open jdk
[root@localhost jdk]# rpm -qa | grep java
java-1.7.0-openjdk-1.7.0.171-2.6.13.0.el6_9.x86_64
tzdata-java-2018c-1.el6.noarch
java-1.6.0-openjdk-1.6.0.41-1.13.13.1.el6_8.x86_64
[root@localhost jdk]#
[root@localhost jdk]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.171-2.6.13.0.el6_9.x86_64
[root@localhost jdk]# rpm -e --nodeps tzdata-java-2018c-1.el6.noarch
[root@localhost jdk]# rpm -e --nodeps java-1.6.0-openjdk-1.6.0.41-1.13.13.1.el6_8.x86_64
[root@localhost jdk]# rpm -qa | grep java
[root@localhost jdk]#

# 安装 JDK 1.8
mv jdk-8u161-linux-x64.tar.gz\?AuthParam\=1521635451_ad06a59373f1aebdda5baa55350799bd jdk-8u161.tar.gz
tar -zxvf /opt/software/packages/jdk-8u161.tar.gz -C /opt/software/sdks/jdk/
```

# 3 配置环境变量
```
vi /etc/profile
```
```
# JAVA_HOME START
JAVA_HOME=/opt/software/sdks/jdk/jdk1.8.0_161
JRE_HOME=$JAVA_HOME/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
# JAVA_HOME END
```
```
source /etc/profile
```
```
[root@localhost jdk1.8.0_161]# java -version
java version "1.8.0_161"
Java(TM) SE Runtime Environment (build 1.8.0_161-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.161-b12, mixed mode)
[root@localhost jdk1.8.0_161]#
```
