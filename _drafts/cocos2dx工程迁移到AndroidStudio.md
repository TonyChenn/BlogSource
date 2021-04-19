---
title: cocos2dx工程迁移到AndroidStudio
date: 2021-04-12 17:17:00
tags:
    - cocos
top:
password:
description:
img: 升级老工程COCOS2dx版本，将Eclipse工程升级到AndroidStudio
---

# 前言

# 升级步骤
## 安装新的COCOS2dx引擎

1. 为了避免最新版出问题，我这里下载了[V3.17.2版本](https://cocos2d-x.org/filedown/cocos2d-x-3.17.2),

2. 配置好环境后，之后创建一个新的工程：

```shell
# project_name    项目名称
# package_name    包名(com.xxx.xxx)
# path            创建路径
cocos new djlw -p package_name -l lua -d path
```
3. 进入工程的`\frameworks\runtime-src\proj.win32\`目录下，双击运行xxx.sln(xxx时上面的项目名称)，`Ctrl+F5`运行，出现报错：

> error MSB8020: 无法找到 Visual Studio 2010 的生成工具(平台工具集 =“v100”)。

解决方法：若要使用 v100 生成工具进行生成，请安装 Visual Studio 2010 生成工具。或者，可以升级到当前 Visual Studio 工具，方式是通过选择“项目”菜单或右键单击该解决方案，然后选择“重定解决方案目标”。

再次运行，出现COCOS的窗口，此时可以确定环境没有问题。