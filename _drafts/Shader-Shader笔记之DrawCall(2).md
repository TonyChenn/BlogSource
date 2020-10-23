---
title: Shader-DrawCall
date: 2020-10-19 15:02：00
tags:
    - Unity
    - Shader
top:
password:
description: 
img:
---

# 什么是DrawCall?
DrawCall就是CPU调用图像变成接口的命令，让GPU进行渲染的命令。

# 为什么DrawCall多了会影响帧率？
CPU与GPU的并行工作应用了<b>并行缓冲区</b>.与生产者-消费者模型类似。CPU提交DrawCall到缓冲区，GPU从缓冲区取数据。
效率上，CPU要向GPU发送许多数据，以及检查工作，而GPU渲染能力很强，所以GPU从缓冲区取命令的速度远远大于CPU发送命令的速度。所以DrawCall过多会导致CPU过载。

# 如何减少DrawCall?
上面说DrawCall过多影响帧率是因为CPU处理数据，发送命令很耗时，所以可以通过合并一些DrawCall,进而减少DrawCall的总数量.

# 开发中减少DrawCall方法
1. 动态字体增加DrawCall
2. 一个界面减少图集数量（NGUI）
3. 图集不穿插（每个图集就是1个DrawCall）
4. UI上的Shader特效会增加1个DrawCall