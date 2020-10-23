---
title: Unity-Application中的各种路径
date: 2020-10-19 12:18:00
tags:
    - Unity
top:
password:
description:
img:
---

# Application.PersistentDataPath
windows下：C:/Users/{USERNAME}/AppData/LocalLow/{CompanyName}/{ProductName}
mac下：
IOS下：
Android下：/storage/emulated/0/Android/data/{PackageName}/

# Application.dataPath
数据路径，即项目的 /Assets/ 目录

# Application.streamingAssetsPath
AssetBundle通常放于此目录，
位置：Application.dataPath+"/StreamingAssets";