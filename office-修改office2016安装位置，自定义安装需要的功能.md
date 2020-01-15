---
title: office-修改office2016安装位置，自定义安装需要的功能
date: 2018-06-22 14:42:27
tags: office
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/icon.png
---

更新：
2019.8.24
解决出现 The product can't be install ont the selection update channel.....无法安装的问题

2019.5.30
楼主本人本次重装系统后装office2019再次测试，没有任何问题,没认真看文章，自己胡乱一同操作，导致各种问题的，出了错就就瞎评论，说博主误导人，对你们这种人就是呵呵。左转不送。

亮点：
配置文件可以由微软官方自行选择配置并到处了，网址：https://config.office.com/deploymentsettings
方便手残党.......

---
---
---
下载新版本office后发现安装过程
**没有自定义安装**
**默认安装路径在C盘**

而我只需要安装Word,PowerPoint,Excel,其他功能都用不着
C盘也快被撑爆了

**步骤：**
（office2019 下载对应镜像就行了）
1.office2016下载地址[https://msdn.itellyou.cn/](https://msdn.itellyou.cn/)
![msdn](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/itellyou.jpg)
下载后解压（我的解压在桌面）

**2.**下载并运行微软提供的[Office2016部署工具](https://www.microsoft.com/en-us/download/confirmation.aspx?id=49117)
会生成2个文件：
	setup.exe
	configuration-xxx.xml
删除所有configuration-xxx.xml文件吧，微软给了新方法：
打开https://config.office.com/deploymentsettings，创建配置文件，并导出到桌面

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/app.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/lan.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/install.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/export.png)


3.实现安装在非系统盘：
Win+R 输入regedit,打开编辑注册表

![cmd_regedit](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/regedit.png)

找到 计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
把下面图片中的三个改成你想要安装的目录:

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/unname1.png)
**到此完成安装目录设置**

4.开始安装：
Win+R输入cmd

```
//打开桌面目录
//就是setup.exe 和 configuration.xml 所在目录
cd desktop

//开始安装
setup.exe /configure configuration.xml

```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/cmd.png)

等待安装完成
。
。
。
安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。
安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。
安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。(重要事情说三遍)

到此安装完成


5.此时点击图标会发现不能运行，你心里肯定在想着，卧槽，博主，你是不是在耍我，不能运行；

淡定，下面开始解决无法运行问题

打开：C:\ProgramData\Microsoft\Windows\Start Menu\Programs\ 目录
依次右键Word,PowerPoint,Excel，点击属性
如图替换成自己的安装目录对应位置
![这里写图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/unname2.png)
完成！



# 问题解决
## The product can't be install ont the selection update channel.....
今天重装按照下面的方法安装后发现执行setup.exe /configure configuration.xml 命令出现无法安装的问题：
![problem_1](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/problem1.png)
仔细看了下配置文件才发现UpdateChanel选择错了，如下没有保持一致，修改下就好了
![在这里插入图片描述](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.22/problem_solve.png)

2018.8.24
没有结果的过程都是垃圾，但偶尔我也会用尽力就好来安慰自己。-------------TonyChenn