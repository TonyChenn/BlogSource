---
title: Xlua-笔记
date: 2020/7/10 15:52
updated: 
tags:
img: 
description:
top: 
---
# 自定义Loader

# Event
```lua
-- 添加点击事件
self:GetComponent("Button").onClick:AddListener(function(){
    -- TODO
})
```
# CSharp访问Lua
## C#访问全局属性
```lua
-- lua代码
name = "TonyChenn"
url ="tonychenn.cn"
age = 23
```
```csharp
//访问字段
string name = luaEnv.Global.Get<string>("name");
int age = luaEnv.Global.Get<int>("age");
```
## C#访问全局table
通过类,结构体，接口映射时，类，结构体，接口中的字段，方法名应与lua中的字段，方法名保持一致。

### 通过类和结构体映射
1. 通过类和结构体是将lua中数据拷贝一份，修改后的字段值不会同步到table，反过来也不会。
2. 暂时没找到通过类（结构体）映射lua中方法，可通过下面接口的方式映射（有大佬知道的，下面留言告知下，谢谢）
3. 映射类不需要添加"CsharpCallLua"特性
4. 开销大，官方不建议使用

```lua
animal={name="Panda", age = 1}
function animal:eat(food){
    print("吃"..food)
}
```
```csharp
//执行lua表
luaEnv.DoString("require 'test'");

//访问表
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");
Animal animal = luaEnv.Global.Get<Animal>("animal");
print("修改前" + animal.name + "\t" + animal.age);
animal.age = 99;
animal.name = "tortoise";
print("修改后" + animal.name + "\t" + animal.age);
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");

public class Animal
{
    public string name;
    public int age;
}
```
输出结果如下：
```log
LUA: lua print: Panda : 1
修改前Panda	1
修改后tortoise	99
LUA: lua print: Panda : 1
```

### 通过接口映射
1. 通过接口会直接修改lua中数据
2. 需要添加“CSharpCallLua”特性，依赖xlua生成代码（如果没生成代码会抛<b>InvalidCastException</b>异常）
```csharp
// lua代码与上面一样

//下面是通过接口访问属性和方法
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");
IAnimal animal = luaEnv.Global.Get<IAnimal>("animal");
print("修改前" + animal.name + "\t" + animal.age);
animal.eat("bamboo");
animal.age = 99;
animal.name = "tortoise";
print("修改后" + animal.name + "\t" + animal.age);
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");

[CSharpCallLua]
public interface IAnimal
{
    string name { get; set; }
    int age { get; set; }
    void eat(string food);
}
```
输出结果
```
LUA: lua print: Panda : 1
修改前Panda	1
LUA: 吃bamboo
修改后tortoise	99
LUA: lua print: tortoise : 99
```

### 映射到Dictionary,List

### 使用by ref方式：映射到LuaTable类
不需要生成代码，但是比较慢，比映射到interface慢一个数量级，没有类型检查

## C#访问全局方法
### 映射到delegate
官方建议，性能好很多，类型安全，但是要生成代码。
### 映射到LuaFunction
优缺点刚好和第一种相反。

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