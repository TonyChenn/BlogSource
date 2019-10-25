---
title: Unity-Unity发布EXE安装程序
date: 2018-09-14 16:07:43
tags: Unity
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/icon.png
---

在我们使用unity打包成Windows平台时候，会生一个.exe的运行文件，一个Data的文件夹，运行软件时必须两个存在并且在同一路径下，那么如何制作成一个像我们在应用市场下载的软件一样只需要一个exe文件便可安装运行呢？

1. 工具：[WinRAR](https://www.rarlab.com/download.htm)或者[好压](https://haozip.2345.cc/)（其他压缩软件没有测试过）

2. 准备两张软件图标，一张 ==.bmp== 格式，用安装包图标；一张 ==.ico== 格式，用作快捷方式图标。最好是正方行的。（.ico文件格式转换，请自行百度，网上有很多在线转换工具）

3. 选中你打包出的.exe文件，Date文件夹和其他等所有文件， 右键选择创建“添加到压缩文件”选项，配置如下图所示：

![zip1](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip1.jpg)

4. 选择“高级”选项，然后点击“自解压选项”。

![zip2](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip2.jpg)

5. 更新选项中勾选解压并替换 覆盖所有文件

![zip3](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip3.jpg)

6. 文本和图标->找到之前准备的两个图标

![zip4](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip4.jpg)

7. 在高级中添加快捷方式

![zip5](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip5.jpg)

![zip6](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/09.14/zip6.jpg)

8.点击确定就能得到压缩好的可安装可运行的.exe文件了.

<div align="right">
TonyChenn

2018.9.14
</div>