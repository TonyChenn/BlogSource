---
title: Unity-编辑器拓展之自定义菜单(1)
date: 2021-01-14 13:38:00
tags: Unity
top:
password:
description:
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/icon.png
---

# 创建菜单(MenuItem)
在Unity引擎顶部工具栏添加按钮：`MenuItem`,需要：
1. 引用命名空间UnityEditor
2. 给方法添加标记
3. 方法必须为静态方法

MenuItem有三个重载方法如下:
```csharp
MenuItem(string itemName)
MenuItem(string itemName, bool isValidateFunction);
MenuItem(string itemName, bool isValidateFunction, int priority)
```
## itemName：菜单的路径和名字
菜单按钮出现的位置可以根据itemName设置的路径来控制，如下表：
|路径|描述|备注|
|---|---|---|
|GameObject/|Hierarchy窗口鼠标右击菜单|优先级设置为1-10|
|Assets/|Project窗口右击菜单||
|CONTEXT/|Inspector窗户右击菜单|

```csharp
[MenuItem("GameObject/拓展工具1")]
static void MyItemMenu1(){}

[MenuItem("Assets/工具1")]
static void MyItemMenu1(){...}

[MenuItem("CONTEXT/工具1")]
static void MyItemMenu1(){...}
```

## isValidateFunction：是否启用

## 从菜单显示优先级
通常情况下自上而下排序，优先级默认都为1000，更改优先级就可进行排序

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/unsort.png)

## 获取当前操作对象
```csharp
[MenuItem("CONTEXT/Camera/绿色背景")]
static void MyItemMenu1(MenuCommand cmd)
{
    Camera camera = cmd.context as Camera;
    camera.backgroundColor = Color.green;
}
```

# 自定义脚本在Inspector面板添加按钮
