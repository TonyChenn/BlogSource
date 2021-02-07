---
title: CSharp反射(0)
date: 2021-2-7 12:15:00
tags: CSharp
top:
password:
description:
img: 反射，反射，程序员的快乐！！！
---

# Unity中使用反射要点
1. 反射在IOS端有限制，最好不使用
2. 如果游戏要发布到IOS, 逻辑中不要使用反射
3. 编辑器拓展工具随便用

# 什么是反射？
- 在运行时检查并使用元数据和编译代码的操作称为反射。
- 反射提供描述程序集、模块和类型的对象（Type 类型）。 
- 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型，然后调用其方法或访问器字段和属性。

# 程序集(Assembly)操作
CSharp编译过后的代码会放到dll或者exe中，通过`Assembly`即可手动加载程序集，并实现对其操作。Unity的运行时代码编译后在`Assembly-CSharp`中，编辑器相关代码在`Assembly-CSharp-Editor`中。
常用加载程序集方法：
```csharp
//
Assembly.Load("AssemblyName")
//根据程序集名称或路径加载,不仅可以加载当前程序集，还可以加载当前所依赖的其他程序集
Assembly.LoadFile("AssemblyPath")
//与LoadFile用法一样，但是只加载当前程序集
Assembly.LoadFrom("AssemblyPath")
```