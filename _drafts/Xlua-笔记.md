---
title: Xlua-笔记
date: 2020/7/10 15:52
updated: 
tags:
img: 
description:
top: 
---

# List<T>列表
## lua描述CSharp列表
```lua
-- csharp列表 List<int>
--lua
local list_rewarditem = CS.System.Collections.Generic.List(CS.UnityGMClient.RewardItem);
```
## lua实例化列表
```lua
local list=list_rewarditem();
```
## 向列表中添加数据
```lua
-- reward 为 RewardItem对象
list:Add(reward);
```

# Lua面向对象编程
## 创建对象
```lua
local reward=CS.UnityGMClient.RewardItem();
```
## 给对象赋值
```lua
reward.m_nItemID=tonumber(10001);   -- 奖励物品ID
reward.m_nCount = tonumber(100);    -- 奖励数量
reward.m_nMatune = tonumber(-1);    -- 奖励物品时效
```