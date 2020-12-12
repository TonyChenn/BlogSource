---
title: xLua笔记之Lua访问CSharp(3)
date: 2020-10-28 19:00:00
tags:
    - Unity
    - Lua
    - xLua
    - 热更新
top:
password:
description:
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1028/icon.jpg
---
1. 再Lua中没有关键字，所有CSharp相关的都在CS下。

# 访问构造方法，静态变量，方法
```lua
local gameobject1 = CS.UnityEngine.GameObject()
-- 获取属性
print(gameobject1.name)
-- 设置属性
gameobject1.name = "GameObject1"
-- 调用方法
gameobject1:SetActive(false)

-- 构造方法
local gameobject2 = CS.UnityEngine.GameObject("GameObject2")

-- 调用静态方法
CS.UnityEngine.Object.DontDestroyOnLoad(gameobject2)
```

# 访问重载方法
由于lua中对于所有数字都是'number'类型,所以lua只支持有限的重载，如参数个数不同。
```csharp
public class Animal
{
    public void SayHello()
    {
        UnityEngine.Debug.Log("Hello");
    }
    public void SayHello(string name)
    {
        UnityEngine.Debug.Log("Hello " + name);
    }
}
```
```lua
local dog = CS.Animal()
dog:SayHello()
dog:SayHello("Cat")
```

# 枚举
```csharp
public enum Direction
{
    Left,Right,Buttom,Top,
}
```
```lua
print(tostring(CS.Direction.Left))
-- 数字转枚举
print(tostring(CS.Direction.__CastFrom(1)))
-- 字符串转枚举
print(tostring(CS.Direction.__CastFrom('Buttom')))

-- 输出
--[[
LUA: Left: 0
LUA: Right: 1
LUA: Buttom: 2
--]]
```

# 访问委托

# 复杂类型与表的转换
```csharp
class A
{
    public int num1;
    public int num2;
}
class B
{
    public A a;
    public int num3;

    public int Add(B b)
    {
        return b.a.num1 + b.a.num2 + b.num3;
    }
}
```
```lua
local _b = CS.B()
local result = _b:Add({a = {num1=11, num2 = 12}, num3 = 13})
print(result)
```