---
title: 算法-回溯之N皇后
date: 2020/7/21 9:27
updated: 
tags:
img: 
description:
top: 
---
# 题目描述
八皇后问题是一个以国际象棋为背景的问题：如何能够在8×8的国际象棋棋盘上放置八个皇后，使得任何一个皇后都无法直接吃掉其他的皇后？为了达到此目的，任两个皇后都不能处于同一条横行、纵行或斜线上。八皇后问题可以推广为更一般的n皇后摆放问题：这时棋盘的大小变为n×n，而皇后个数也变成n。当且仅当n = 1或n ≥ 4时问题有解([出自维基百科](https://zh.wikipedia.org/wiki/%E5%85%AB%E7%9A%87%E5%90%8E%E9%97%AE%E9%A2%98))

# 以前写的暴力解法：
![nqueue_exaustive_attack](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0722/nqueue_exaustive_attack.jpg)
# 回溯解法
