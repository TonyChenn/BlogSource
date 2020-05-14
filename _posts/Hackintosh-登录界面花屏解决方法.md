---
title: Hackintosh-登录界面花屏解决方法
date: 2019-06-14 19:32:14
tags: Hackintosh
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0614/mojave.jpg
description:
top:
---

# 问题介绍
昨天把EFI改坏进不去系统，今天替换以前备份的EFI,发现在开机第二阶段出现“八苹果”，并且解锁界面直接花的惨不忍睹。可以通过长按电源键3-4秒后恢复正常，但是不能每次都这样吧。所以经过爬贴问大佬，最终通过在Clover注入edid解决。

# 详细步骤
- 进Windows下载 EDIDManger.exe(自行百度)，安装运行；
- 选择Load EDID from Registry,之后不更改DisplayID,直接点击OK.
- 点击Save edid report,并保存到桌面。
- 用Notepad打开，并删除没用的消息，留下如下格式的内容：
```
00FFFFFFFFFFFF004C2D
2C0D564C50302E190103
80341D782A5295A55654
9D250E5054BB8C00B300
81C0810081809500A9C0
01010101023A80187138
2D40582C450009252100
001E000000FD0032481E
5111000A202020202020
000000FC004332344633
39300A20202020200000
00FF004854514A433030
3037370A20200137
```
- 再删除每行间的换行，如下：
```
00FFFFFFFFFFFF004C2D2C0D564C50302E19010380341D782A5295A556549D250E5054BB8C00B30081C0810081809500A9C001010101023A801871382D40582C450009252100001E000000FD0032481E5111000A202020202020000000FC00433234463339300A2020202020000000FF004854514A4330303037370A20200137
```
- 切换到Mac,使用Clover Configerator注入edid
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0614/inject-edid.png)

完成。


# 注意
请自行提取edid,使用别人分享的可能导致不开机！！！