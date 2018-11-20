---
title: 安装_CENTOS_ROCKET_MQ
date: 2018-11-19 18:29:49
categories: [技术]
tags: [安装,CENTOS]
---

# 文档
* [官方文档](http://rocketmq.apache.org/docs/quick-start/)
* [参考文档](https://blog.csdn.net/u013680938/article/details/53447996)

# 安装
```
# 64bit OS, Linux/Unix/Mac is recommended
# install 64bit JDK 1.8+
# install Maven 3.2.x

yum install git

cd /opt/software
git clone -b release-4.2.0 https://github.com/apache/rocketmq.git
cd rocketmq
mvn -Prelease-all -DskipTests clean install -U
```
* 环境变量
```
vi /etc/profile
```
```
# ROCKETMQ_HOME
ROCKETMQ_HOME=/opt/software/rocketmq/distribution/target/apache-rocketmq

# MAVEN_HOME
MAVEN_HOME=/opt/software/apache-maven-3.5.3

# JAVA_HOME START
JAVA_HOME=/opt/software/sdks/jdk/jdk1.8.0_162
JRE_HOME=$JAVA_HOME/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$MAVEN_HOME/bin:$ROCKETMQ_HOME:$ROCKETMQ_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
# JAVA_HOME END
```
```
source /etc/profile
```

# Start Name Server
```
  > cd /opt/software/rocketmq/distribution/target/apache-rocketmq
  > nohup sh bin/mqnamesrv &
  > tail -f ~/logs/rocketmqlogs/namesrv.log
  The Name Server boot success...
```

# Start Broker
```
  > cd /opt/software/rocketmq/distribution/target/apache-rocketmq
  > nohup sh bin/mqbroker -n localhost:9876 &
  > tail -f ~/logs/rocketmqlogs/broker.log 
  The broker[%s, 172.30.30.233:10911] boot success...
```

# Shutdown Servers
```
> cd /opt/software/rocketmq/distribution/target/apache-rocketmq
> sh bin/mqshutdown broker
The mqbroker(36695) is running...
Send shutdown request to mqbroker(36695) OK

> sh bin/mqshutdown namesrv
The mqnamesrv(36664) is running...
Send shutdown request to mqnamesrv(36664) OK
```

# Send & Receive Messages
```
 > cd /opt/software/rocketmq/distribution/target/apache-rocketmq
 > export NAMESRV_ADDR=localhost:9876
 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
 SendResult [sendStatus=SEND_OK, msgId= ...

 > cd /opt/software/rocketmq/distribution/target/apache-rocketmq
 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
 ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```

# 防火墙
```
# 开放 9876 端口
```

# 遇到的问题
* 启动 Name Server 和 Broker 的时候 报内存不足
```
“VM warning: INFO: OS::commit_memory(0x00000006c0000000, 2147483648, 0) faild; error=’Cannot allocate memory’ (errno=12)”
```
* 解决方案：修改/RocketMQ/devnev/bin/ 下的服务启动脚本 runserver.sh 、runbroker.sh 中对于内存的限制，​改成如下示例：
```
 JAVA_OPT="${JAVA_OPT} -server -Xms128m -Xmx128m -Xmn128m -XX:PermSize=128m -XX:MaxPermSize=128m"
```
