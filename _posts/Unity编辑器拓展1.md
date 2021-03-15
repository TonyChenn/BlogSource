---
title: Unity编辑器拓展1
date: 2021-2-26 20:08:00
tags:
    - Unity
    - UnityEditor
top:
password:
description: 
img:
---
# 介绍
编辑器拓展，是Unity中通过自定义Inspector和自定义窗口的形式进行对unity功能上的拓展。通过编辑器拓展可以帮助实现一些方便开发的功能。

# 注意
1. Unity编辑器相关脚本必须放在`Editor`文件夹下，此文件夹在工程中不限位置和个数。
2. `Editor`目录下的文件编译后存放在`Assembly-CSharp-Editor.dll`中。
3. `Editor`文件夹在打包后回被剔除掉，不会打包进工程。
4. 在运行时的代码(相对编辑器相关代码)中，如果用到了编辑器相关资源，在编辑器下不会报错，但是打包过程会报错。

# 编辑器默认资源文件夹
工程根目录创建`Editor Default Resources`文件夹，可以通过`EditorGUIUtility.Load (path)`加载资源