---
title: Hackintosh升级Mojave后花屏问题
date: 2019-04-05 11:31:24
tags: Hackintosh
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0405/clover.jpg
---

# 前言
今天将Hackintosh 从Mojave10.14.3升级至10.14.4后发现出现屏幕花屏的情况，经过各种查阅，发现是因为显存出现的问题。虽然在10.14.4版本中该方法无法将显存提升到2048，但还是解决了花瓶的问题，博主解决花屏问题后，记录一下，希望也能帮助你尽快解决花屏问题。

# 配置
显卡： HD4600

# 开始
## 查看ig-platform-id

```commond
ioreg -l | grep ig-platform-id
```
可以看到我的ig-platform-id是：0600260a

![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0405/ig.jpg)

## 查看当前加载的Framebuffer

```commond
kextstat | grep -y AppleIntel
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0405/framebuffer.jpg)

找到含有"Framebuffer"的一项，我的是：AppleIntelFramebufferAzul

在/System/Library/Extensions/文件夹下找到含有AppleIntelFramebufferAzul.kext的驱动，右键显示包内容，在 /Contents/MacOS 下将 kext 的文件拷贝到桌面，并在AppStore下载HexFriend打开该文件，如下：

![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0405/find.jpg)

commond+F进行查找上面的ig-platform-id：直到找到连续的一组，并且下一组是：01030303：

![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/0405/find1.jpg)

复制01030303及后面的四组，然后添加到config.plist的Kernel and Kext Pathes->KextsToPatch中：

```
Name：           AppleIntelFramebufferAzul
Find：           01030303 00000002 00003001 00006000 00000060
Replace：        01030303 00000002 00003001 00009000 00000080
Comment：        随意
```
## 重建缓存，重启，完成。
