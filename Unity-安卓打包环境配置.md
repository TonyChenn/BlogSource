---
title: Unity-安卓打包环境配置
date: 2018-10-13 09:50:24
tags: Unity
top:
password:
---
![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6ezxvhs0j30xf0got9g.jpg)
<!-- more -->

当前Unity版本2018.2.8f1
# 1.下载JDK：
[JDK下载地址](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6d21xex5j30fd094jt0.jpg)

## 1.1环境配置自行百度

# 2.下载SDK:
[需要翻墙(官网)](https://developer.android.com/sdk/index.html)
[不需要翻墙](https://www.androiddevtools.cn/)

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6d99yqlsj311y0aeabv.jpg)

## 2.1 解压到需要安装的目录
## 2.2 双击打开SdkManger，按照下面配置进行勾选

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6dj3ttiuj30h002jmx6.jpg)

API自行选择，建议 最新 + 4.4

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6dmmsul1j30hk06tt9f.jpg)

## 2.3 配置环境变量
> 变量名：ANDROID_SDK_HOME <br>
> 变量值: SDK安装路径

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6e4lssrjj30ij05a3yj.jpg)

# 3.配置Unity环境
## 3.1 file->build setting...

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6dsqf3ltj30hh09v3zr.jpg)

修改 Other Setting中的 Package Name(此处不改包名，在打包时会报错)

![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6duzkxtij30au0ctwf3.jpg)

## 3.2 Edit—Preferences—External Tools
选择对应SDK,JDK目录
![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw6dzqaihcj30fg0c7dgl.jpg)

# 遇到错误

## 1. Unable to retrieve device properties. Please make sure the Android SDK is installed and is properly configured in the Editor. 

解决方案：打开手机USB调试,打开USB安装 即可继续食用

参考
> https://docs.unity3d.com/Manual/android-sdksetup.html
https://blog.csdn.net/dxj467822057/article/details/80198165

</br>
</br>
<div align="right">---2018.10.13</br>TonyChenn</div>