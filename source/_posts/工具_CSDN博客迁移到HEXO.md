---
title: 工具_CSDN博客迁移到HEXO
date: 2018-11-19 18:29:49
categories: [技术]
tags: [工具]
---

### 介绍
将自己CSDN 上的所有博客迁移到 hexo 博客里面

### 第一步 初始化脚本环境
git clone [https://github.com/krmao/spider_csdn.git](https://github.com/krmao/spider_csdn)
自己初始化好 python 环境, 安装好 scrapy
```
$ python
Python 2.7.10 (default, Feb  7 2017, 00:08:15)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### 第二步 爬取所有的博客链接到 list.json 文件
计算自己在 csdn 一共有多少篇博客 [https://blog.csdn.net/mkrcpp/](https://blog.csdn.net/mkrcpp/) 假如有 100篇, 页数就写 100/20=5, 但是多写一点比较保险, 比如写 10
修改 csdn-list.py
```$xslt
pageCount = 10
```
将所有的用户名改成你的
```
for pageNum in range(1,pageCount+1):
    urls.append("https://blog.csdn.net/mkrcpp/article/list/"+str(pageNum))
if "mkr127" in link or "mkrcpp" in link: # 你可能修改过用户名, 都写在这里
```

### 第三步 根据刚刚 list.json 开启另一个爬虫爬取所有的文章
修改 csdn-detail.py
修改生成的文件名, 以及文章的 标签 分类等
```
mdFileName = "./blogs/移动端_"+title.replace("【","").replace("】","_").replace(" ","_").replace("〖","").replace("〗","")+".html"
contentPrefix =  "---"+ "\ntitle: 移动端_IOS"+title+"\ndate: "+time+"\ncategories: [技术]"+ "\ntags: [移动端]"+ "\n---\n"
```

### 第四步 开始执行
1. 爬取所有链接到 list.json
```
scrapy crawl csdn-list
```
生成后的 list.json
```
[
    "https://blog.csdn.net/mkr127/article/details/8849365", 
    "https://blog.csdn.net/mkr127/article/details/8845499", 
    "https://blog.csdn.net/mkr127/article/details/12218833", 
    "https://blog.csdn.net/mkr127/article/details/8941659", 
    "https://blog.csdn.net/mkr127/article/details/44222117", 
    "https://blog.csdn.net/mkr127/article/details/41988471", 
    "https://blog.csdn.net/mkr127/article/details/41987633", 
    "https://blog.csdn.net/mkr127/article/details/40783857", 
    "https://blog.csdn.net/mkr127/article/details/40377547", 
    "https://blog.csdn.net/mkr127/article/details/39889389", 
    "https://blog.csdn.net/mkr127/article/details/39320797", 
    "https://blog.csdn.net/mkr127/article/details/39181077", 
    "https://blog.csdn.net/mkr127/article/details/39178213", 
    "https://blog.csdn.net/mkr127/article/details/39153951", 
    "https://blog.csdn.net/mkr127/article/details/39040479", 
    "https://blog.csdn.net/mkr127/article/details/38894431", 
    "https://blog.csdn.net/mkr127/article/details/20993533", 
    "https://blog.csdn.net/mkr127/article/details/14166627", 
    "https://blog.csdn.net/mkr127/article/details/14166121", 
    "https://blog.csdn.net/mkr127/article/details/13509051", 
    "https://blog.csdn.net/mkr127/article/details/13171587", 
    "https://blog.csdn.net/mkr127/article/details/13170649", 
    "https://blog.csdn.net/mkr127/article/details/8810233", 
    "https://blog.csdn.net/mkr127/article/details/8630659", 
    "https://blog.csdn.net/mkr127/article/details/8432851", 
    "https://blog.csdn.net/mkr127/article/details/8053252", 
    "https://blog.csdn.net/mkr127/article/details/7802162", 
    "https://blog.csdn.net/mkr127/article/details/7785801", 
    "https://blog.csdn.net/mkr127/article/details/7766473", 
    "https://blog.csdn.net/mkr127/article/details/7761546", 
    "https://blog.csdn.net/mkr127/article/details/7731425", 
    "https://blog.csdn.net/mkr127/article/details/7728913", 
    "https://blog.csdn.net/mkr127/article/details/12995615", 
    "https://blog.csdn.net/mkr127/article/details/12179847", 
    "https://blog.csdn.net/mkr127/article/details/12090695", 
    "https://blog.csdn.net/mkr127/article/details/12089871", 
    "https://blog.csdn.net/mkr127/article/details/12036463", 
    "https://blog.csdn.net/mkr127/article/details/12031509", 
    "https://blog.csdn.net/mkr127/article/details/11993163", 
    "https://blog.csdn.net/mkr127/article/details/11993007", 
    "https://blog.csdn.net/mkr127/article/details/11992647", 
    "https://blog.csdn.net/mkr127/article/details/11992317", 
    "https://blog.csdn.net/mkr127/article/details/11992025", 
    "https://blog.csdn.net/mkr127/article/details/11984875", 
    "https://blog.csdn.net/mkr127/article/details/11646547", 
    "https://blog.csdn.net/mkr127/article/details/11563083", 
    "https://blog.csdn.net/mkr127/article/details/11522657", 
    "https://blog.csdn.net/mkr127/article/details/11478011", 
    "https://blog.csdn.net/mkr127/article/details/9792191", 
    "https://blog.csdn.net/mkr127/article/details/9313411", 
    "https://blog.csdn.net/mkr127/article/details/8933891", 
    "https://blog.csdn.net/mkr127/article/details/8930381"
]
```

2. 根据list.json爬取所有的文章
```
scrapy crawl csdn-detail
```
生成后的所有文章
./blogs 目录
```
移动端_Android框架进阶00_ThinkAndroid注解机制.html
移动端_Android框架进阶01_ThinkAndroid注解机制二.html
移动端_Android框架进阶02_ThinkAndroid线程池机制.html
移动端_Android框架进阶03_ThinkAndroid框架总结.html
移动端_Android框架进阶04_MDevil_之_网络模块_Volley.html
移动端_Android基础入门0_AndroidStudio.html
移动端_Android基础入门10_BaseAdapter中convertView回收的机制.html
移动端_Android基础入门11_Fragment.html
移动端_Android基础入门12_滑动菜单SlidingMenu.html
移动端_Android基础入门13_Gallery.html
移动端_Android基础入门14_SharedPreferences.html
移动端_Android基础入门15_Shape圆角输入框.html
移动端_Android基础入门16_XML解析.html
移动端_Android基础入门17_自定义标签_和_自定义组件.html
移动端_Android基础入门18_新浪微博项目总结.html
移动端_Android基础入门19_ExpandableListView酷我音乐界面的下拉菜单.html
移动端_Android基础入门1_UI布局.html
移动端_Android基础入门2_Eclipse_导入_Android_源码.html
移动端_Android基础入门3_四大组件之Activity.html
移动端_Android基础入门4_四大组件之Service.html
移动端_Android基础入门5_四大组件之BroadcastReceiver.html
移动端_Android基础入门6_四大组件之ContentProvider.html
移动端_Android基础入门7_SurfaceView坦克大战之世界地图.html
移动端_Android基础入门8_SimpleAdapter之一.html
移动端_Android基础入门9_SimpleAdapter之二.html
移动端_Android框架进阶_我有一个愿望.html
移动端_Django基础入门_Aptana提示功能和shell设置.html
移动端_Django基础入门_Breadcrumbs导航栏.html
移动端_Django基础入门_Ckeditor_Wins7下图片上传及显示的路径问题.html
移动端_Django基础入门_ManyToManyField跨越中间表查询.html
移动端_Django基础入门_生产环境搭建.html
移动端_Django基础入门_空间数据库查询.html
移动端_IOS编程_00_The_Swift_Programming_Language.html
移动端_IOS编程_01_开发环境.html
移动端_IOS编程_02_加法器.html
移动端_Java服务端编程00_是时候开始了.html
移动端_Java服务端编程01_搭建环境第一步.html
移动端_MFC基础入门_LabJack的研究.html
移动端_MFC基础入门_OpenCV人脸检测与马赛克.html
移动端_MFC基础入门_基于Adaboost算法的车牌检测在OpenCV上的研究与实现.html
移动端_PHP基础入门_配置Phpstorm与xampp.html
移动端_TextView_设置无下划线超链接.html
移动端_谷歌地图_DEMO.html
移动端_心情日记_从这里开始吧.html
移动端_心情日记_总有一天你将破蛹而出.html
移动端_有趣的面试题_有人邀请ABCDEF六人参加一项会议.html
移动端_数据结构与算法_00_二分插入排序.html
移动端_数据结构与算法_01_冒泡排序.html
移动端_数据结构与算法_02_选择排序.html
移动端_数据结构与算法_03_直接插入排序.html
移动端_数据结构与算法_二叉树.html
移动端_数据结构与算法_图的创建与遍历.html
```

### 部署到hexo 
copy 这些博客到 hexo的 source/_posts 目录
```
hexo g && hexo d
```
### 预览地址
[https://krmao.github.io/tags/mobile/](https://krmao.github.io/tags/mobile/)