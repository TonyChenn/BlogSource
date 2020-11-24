---
title: Unity-ScrollView中Item加连线
date: 2020-11-12 15:29:00
tags:
    - Unity
    - UGUI
top:
password:
description: UGUI下ScrollView中Item的连线
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1112/icon.jpg
---

# 目的
制作的一个培养模块，需要一级一级的升，UI上需要根据配表读取Item的位置，将Item显示出来，同时将两个Item直接用连线连接起来。效果图如下:
 ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1112/scrollwithline.gif)

# 实现原理
1. 计算两个Item之间的距离作为连线的长度
```lua
local distance= Vector3.Distance(pointA, pointB)
```
2. 计算连个Item的重点作为连线的中点
```lua
local temp=pointB - pointA
local centerPos = pointA + Vector3(temp.x/2, temp.y/2, 0)
```
2. 根据两个Item计算连线的旋转
```lua
local rotate = (PointB-PointA).normalized
```

# 具体使用
```lua
-- 连线
-- tLength -> 要连线Item的个数
for i = 1, tLength-1 do
    -- 距离，中点，角度
    local distance, centerPos, rotate = self:PointInfo(self._cachedItemsPos[i], self._cachedItemsPos[i+1])
    
    local trans = self._cachedLines[i]:GetComponent("RectTransform")
    trans.sizeDelta = Vector2(distance, trans.sizeDelta.y)
    self._cachedLines[i].transform.localPosition = centerPos
    self._cachedLines[i].transform.right = rotate
    CUIHelper.SetActive(self._cachedLines[i], true)
end
```

# 遇到问题
由于里面Item的位置是自定义的，Line也是自定义位置，旋转，大小的，所以再UGUI中的ScrollView中不能使用`HorizontalLayoutGroup`或者`VerticalLayoutGroup`以及`GridLayoutGroup`,但是不使用这些控件会导致ScrollView没办法滚动，原因是滚动区域的大小与所有Item的大小不一致。如下图所示：
![cantscroll](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1112/cantscroll.jpg)

既然无法自动改变滚动区域大小，那就手动设置下滚动区域大小即可。