---
title: 安装_CENTOS_ANDROID_SDK
date: 2018-11-19 18:29:49
categories: [技术]
tags: [安装,CENTOS]
---

# 1 下载地址 [https://developer.android.com/studio/index.html#downloads](https://developer.android.com/studio/index.html#downloads)
```
cd /opt/software/packages/
wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
```

# 2 安装
* 解压到指定目录
```
unzip /opt/software/packages/sdk-tools-linux-3859397.zip -d /opt/software/sdks/android/sdk
cd /opt/software/sdks/android/sdk/tools/bin
```
* 检查需要安装的 sdk 版本
```
// Sdk and tools
minSdkVersion = 15
targetSdkVersion = 25
compileSdkVersion = 27
buildToolsVersion = "27.0.3"

// App dependencies
supportLibraryVersion = "27.0.2"
constraintLayoutVersion = "1.0.2"
multidex = "1.0.2"
```
* 执行安装命令
```
./sdkmanager --help

# echo y | 通过管道模式直接确认,不需要等会儿手动输入 y 确认
echo y | ./sdkmanager "build-tools;27.0.3" "platforms;android-27" "platform-tools" "ndk-bundle" "extras;android;m2repository" "extras;google;m2repository" "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.2" "tools"
```
* 默认安装的目录结构 
```
# /opt/software/sdks/android/sdk
[root@localhost android]# tree -L 3
.
└── sdk
    ├── build-tools
    │   └── 27.0.3
    ├── emulator
    │   ├── bin64
    │   ├── emulator
    │   ├── emulator64-arm
    │   ├── emulator64-crash-service
    │   ├── emulator64-mips
    │   ├── emulator64-x86
    │   ├── emulator-check
    │   ├── lib
    │   ├── lib64
    │   ├── mksdcard
    │   ├── NOTICE.txt
    │   ├── package.xml
    │   ├── qemu
    │   ├── resources
    │   └── source.properties
    ├── extras
    │   ├── android
    │   ├── google
    │   └── m2repository
    ├── licenses
    │   └── android-sdk-license
    ├── ndk-bundle
    │   ├── build
    │   ├── CHANGELOG.md
    │   ├── meta
    │   ├── ndk-build
    │   ├── ndk-depends
    │   ├── ndk-gdb
    │   ├── ndk-stack
    │   ├── ndk-which
    │   ├── package.xml
    │   ├── platforms
    │   ├── prebuilt
    │   ├── python-packages
    │   ├── README.md
    │   ├── shader-tools
    │   ├── simpleperf
    │   ├── source.properties
    │   ├── sources
    │   ├── sysroot
    │   └── toolchains
    ├── patcher
    │   └── v4
    ├── platforms
    │   └── android-27
    ├── platform-tools
    │   ├── adb
    │   ├── api
    │   ├── dmtracedump
    │   ├── e2fsdroid
    │   ├── etc1tool
    │   ├── fastboot
    │   ├── hprof-conv
    │   ├── lib64
    │   ├── make_f2fs
    │   ├── mke2fs
    │   ├── mke2fs.conf
    │   ├── NOTICE.txt
    │   ├── package.xml
    │   ├── sload_f2fs
    │   ├── source.properties
    │   ├── sqlite3
    │   └── systrace
    └── tools
        ├── android
        ├── bin
        ├── emulator
        ├── emulator-check
        ├── lib
        ├── mksdcard
        ├── monitor
        ├── NOTICE.txt
        ├── package.xml
        ├── proguard
        ├── source.properties
        └── support

38 directories, 42 files
[root@localhost android]#
```
# 设置环境变量
```
vi /etc/profile

# ANDROID_HOME START
ANDROID_HOME=/opt/software/sdks/android/sdk
PATH=$PATH:$ANDROID_HOME:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:ANDROID_HOME/emulator
export ANDROID_HOME PATH
# ANDROID_HOME END

source /etc/profile
```

# 遇到的问题
* Unsupported major.minor version 52.0 [点击查看如何安装 JDK1.8](https://www.jianshu.com/p/9cde35b7faba)
```
# 需要安装 jdk 1.8 版本
JDK 1.8 = 52
JDK 1.7 = 51
JDK 1.6 = 50
JDK 1.5 = 49
JDK 1.4 = 48
JDK 1.3 = 47
JDK 1.2 = 46
JDK 1.1 = 45
```

* 编译 android 项目 报错 **'java.lang.RuntimeException: No server to serve request. Check logs for details.'**
```
# 仔细查看编译日志
AAPT err(Facade for 232115523): /opt/software/sdks/android/sdk/build-tools/27.0.3/aapt: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /opt/software/sdks/android/sdk/build-tools/27.0.3/aapt)
AAPT err(Facade for 232115523): /opt/software/sdks/android/sdk/build-tools/27.0.3/aapt: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /opt/software/sdks/android/sdk/build-tools/27.0.3/lib64/libc++.so)

# 查看系统是否已经安装 glibc 2.14
strings /lib64/libc.so.6 |grep GLIBC

# 发现不包含 2.14, 安装 glibc 2.14 版本应该可以解决问题
yum install gcc g++

cd /opt/software/packages/ && wget wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
tar zxvf glibc-2.14.tar.gz
cd glibc-2.14
mkdir build
cd build
../configure –prefix=/opt/glibc-2.14
make -j4
make install

# /opt/software/packages/glibc-2.14/build/elf/ldconfig: Can't open configuration file /opt/software/glibc-2.14/etc/ld.so.conf: No such file or directory
cp /etc/ld.so.conf /opt/software/glibc-2.14/etc/
make install

# make install 最后报 ld.so.conf: No such file or directory
mkdir -p $prefix/etc
touch $prefix/etc/ld.so.conf
make install

# 重新查看系统是否已经安装 glibc 2.14
strings /lib64/libc.so.6 |grep GLIBC
# 我这里是仍然没有看到 2.14 :) 哈哈

# 修改 aapt
cd /opt/software/sdks/android/sdk/build-tools/27.0.3
mv ./aapt ./aapt_
vi ./aapt

#!/bin/sh
export LD_LIBRARY_PATH=/opt/software/glibc_2_14/lib && "$0"_ $@

chmod +x ./aapt

# 对 ./aapt2 也做相同的处理,因为发现有的时候编译使用的是 aapt, 有的时候使用的是 aapt2

# 重新build android 项目
./gradlew clean assembleDebug --stacktrace

# 编译成功 !!!
```
