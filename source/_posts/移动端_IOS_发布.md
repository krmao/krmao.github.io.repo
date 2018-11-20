---
title: 移动端_IOS_发布
date: 2018-11-19 18:29:49
tags: [移动端,IOS]
---

### 1: 首先设置 apple [开发者账号](https://developer.apple.com/account/ios/identifier/bundle])
我把所有的(Certificates/Provisioning Profiles)全部删除了,只留下 Identifiers 菜单里面的 App IDs 里面的  该应用的 id

![image.png](http://upload-images.jianshu.io/upload_images/2062311-b2e1aa094039036c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2: xcode 登录账号密码
### 3: Targets -> General -> Identity -> Bundle Identifier  输入 与步骤一里面 app id 对应的 id
### 3: Targets -> General -> Signing -> Team 选择注册 该应用 Bundle Identifier 的账号 (这里很关键)
* 一般个人账号没有注册 Bundle Identifier (Personal Team 都是为了自己免费在自己的真机上测试使用的)
* 如果选择了注册 该应用 的 Bundle Identifier 的账号,则 步骤二必须 填写相同的 Bundle Identifier,生成的包应该即可以发布到appstore,也可以发给注册了 iphone-uuid 的用户安装测试
* 如果选择了个人账号(Personal Team ),则必须填写与 appstore中完全不重复的 Bundle Identifier(随便写一个)才能编译通过,而且生成的包不能发布到 app store, 但是应该依然可以发给注册了 iphone-uuid 的用户安装使用
### 4: Targets -> Build Settings -> Code Signing Identity -> 选择步骤3里面的 Signing Certificate 相同账号的账号
### 5: Xcode -> Product -> Archive 进行打包
* 如果个人/公司账号想要发布到 蒲公英等,打包成功后选择 AD HOC
![发布 ad hoc.png](http://upload-images.jianshu.io/upload_images/2062311-d539bd819e46c5b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 如果想发布到 app store 则直接点击 upload to appstore
![发布.png](http://upload-images.jianshu.io/upload_images/2062311-b4bd45902c958451.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

