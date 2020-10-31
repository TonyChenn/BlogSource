---
title: xLua-笔记
date: 2020-07-10 15:52:00
updated: 2020-10-26
tags:
    - Unity
    - Lua
    - xLua
    - 热更新
img: 
description:
top: 
---
# 执行Lua
```csharp
//直接执行lua字符串
luaenv.DoString("print('hello world')")

//执行lua文件
//xlua会从原生loader中以及Resources中加载下面lua文件，如果都找不到就会报错。由于Resources文件夹不支持.lua 文件，所以如果要把lua文件放Resources中时，要加后缀为.txt，当然xLua官方也支持从其他地方加载lua文件，就是通过下面的自定义Loader来实现.
luaEnv.DoString("require 'lua_file_name'");
```
# 自定义Loader
```
```
# Event
```lua
-- 添加点击事件
self:GetComponent("Button").onClick:AddListener(function(){
    -- TODO
})
```

# Lua调用C#

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