---
title: Unity-资源管理
date: 2020/9/14 11:30
tags:
---
# Unity打包后资源所在位置及特性
## 游戏逻辑相关脚本
打包后会放进Assembly-CSharp.dll中，
## 编辑器相关脚本

# Resources目录
打包后会对该目录下文件进行处理（不以原文件存在）

# StreammingAsset目录
放在StreammingAsset目录下的文件会在assets目录下已原文件的形式存在，Unity不会对其进行操作。

# Application.presistentDataPath
放在该目录下的资源可读可写