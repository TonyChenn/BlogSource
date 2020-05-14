---
title: Unity-Jenkins自动化打包笔记
date: 2020-05-07 09:15:27
tags:
    - Unity
    - Jenkins
img:
description:
top:
---
# environment config
1. 下载安装[jenkins](https://www.jenkins.io/download/)
2. Java 8 or 11 (either a JRE or Java Development Kit (JDK) is fine)
3. 

# 常用命令 Jenkins
前往Jenkins安装目录使用CMD命令运行下面命令：
1. 开启Jenkins
```bush
net start jenkins
```
2. 关闭Jenkins
```bush
net stop jenkins
```


# 插件使用
## 切换本地化语言
1. 在插件管理界面，下载locale 插件安装;
2. 下载 == Localization: Chinese (Simplified) ==插件
3. 重启




# 问题解决
## Jenkins没有设置账号无法登录
如果没有修改密码，则账号使用 admin，密码默认为：Jenkins安装目录下secrets\initialAdminPassword文件中

## Jenkins忘记密码
1. 进入Jenkins安装目录，打开config.xml，删除下面代码：
```
```