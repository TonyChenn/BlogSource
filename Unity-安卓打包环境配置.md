---
title: Unity-安卓打包环境配置
date: 2018-10-13 09:50:24
tags: Unity
top:
password:
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0426/icon.png
---

当前Unity版本2018.2.8f1
# 1.下载JDK：
[JDK下载地址](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

![jdk](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/jdk.jpg)

## 1.1环境配置自行百度

# 2.下载SDK:
[需要翻墙(官网)](https://developer.android.com/sdk/index.html)
[不需要翻墙](https://www.androiddevtools.cn/)

![sdk](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/sdk.jpg)

## 2.1 解压到需要安装的目录
## 2.2 双击打开SdkManger，按照下面配置进行勾选

![sdkmanger1](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/sdkmanger1.jpg)

API自行选择，建议 最新 + 4.4

![sdkmanger2](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/sdkmanger2.jpg)

## 2.3 配置环境变量
> 变量名：ANDROID_SDK_HOME <br>
> 变量值: SDK安装路径

![sdkhome](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/sdkhome.jpg)

# 3.配置Unity环境
## 3.1 file->build setting...

![build](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/build.jpg)

修改 Other Setting中的 Package Name(此处不改包名，在打包时会报错)

![setting](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/setting.jpg)

## 3.2 Edit—Preferences—External Tools
选择对应SDK,JDK目录
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/10.13/config.jpg)

# 遇到错误

## 1. Unable to retrieve device properties. Please make sure the Android SDK is installed and is properly configured in the Editor. 

解决方案：打开手机USB调试,打开USB安装 即可继续食用

参考
> https://docs.unity3d.com/Manual/android-sdksetup.html
https://blog.csdn.net/dxj467822057/article/details/80198165

</br>
</br>
<div align="right">---2018.10.13</br>TonyChenn</div>