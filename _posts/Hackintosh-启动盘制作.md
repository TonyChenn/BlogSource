---
title: Hackintosh-启动盘制作(一)
date: 2019-02-27 10:39:09
tags: Hackintosh
top:
password:
description:
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0614/mojave.jpg
---

# 1.Windows环境下:

## 软件下载：
- [TransMac](https://pan.baidu.com/s/1gr7tP_DEAqwOkMP4OzXfAQ)
 密码:qk64
- [DiskGenius](https://pan.baidu.com/s/1xPD-YAqwpoWV3cmIcs55zg) 密码:vvf4
- Mac OS系统镜像(推荐前往[黑果小兵的部落阁](https://blog.daliansky.net)寻找合适版本进行下载

## 物品准备：
- windows电脑一台
- U盘两个（一个mac做启动盘，另一个备用）
- 一颗折腾的💖

## 制作流程
1. 安装TransMac并打开，使用TransMac将硬盘格式化为Mac支持的格式：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0227/format.jpg)

2. 选择下载的Mac镜像写入U盘：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0227/write.jpg)

- 到此，你的U盘在Windows下就不能正常识别了，同时windows也会提示“此U盘无法识别，请格式化U盘”之类的，此时要选择取消格式化！

3.修改EFI分区：
- 打开DiskGenius，将UFI盘中的EFI文件夹复制到桌面；
- 将EFI盘格式化为MS格式，再将EFI文件夹拷贝到EFI磁盘内，到此，启动盘制作成功。



# 2.Mac环境下:

比较麻烦，博主本人是在win下制作的，没有经验就不写了，如果你有经验欢迎提交给我。


<div align="right">TonyChenn<br> 2019.2.27</div>