---
title: NGUI-UITexture变灰shader(一)
date: 2020-08-20 16:11:00
updated: 
tags:
    - Unity
    - Shader
    - NGUI
img: 
description:
top: 
---
# 前言
最近在做一个闯关解锁关卡的功能，对于未解锁的关卡尽心灰度处理，解锁后变为彩色。因为会有许多关卡，所以让美术提供彩图以及对应的灰度图也不现实，毕竟要增加一倍的图片资源。所以在google后使用Shader的方式实现图片变灰。
# 原理
## 公式
灰色值=颜色.r x 0.299 + 颜色.g x 0.587 + 颜色.b * 0.114
# 遇到问题
1. 在UIScrollView中使用soft clip无法剪裁.
