---
title: Android-生成keystore密钥
date: 2018-02-06 11:43:34
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/02.06/androidstudio.jpg
---

打开cmd 进入Jdk的 安装目录下的bin文件夹；

 输入命令：
 ```cmd
 keytool -genkey -alias android.keystore -keyalg RSA -validity 20000 -keystore android.keystore
```
 20000为有效期20000天；

 回车输入密码，等各种信息；

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/02.06/createKeystore.jpg)


 之后会在bin\ 下生成android.keystore

 2018.2.6

 TonyChen
