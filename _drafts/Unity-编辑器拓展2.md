---
title: Unity-编辑器拓展(Attribute)
date: 2021/1/13 20:56
tags:
top:
password:
description:
img:
---

# 常用Attribute
|Attribute|作用|
|---|---|
|HideInInspector|在属性面板隐藏|
|Tooltip("")|鼠标放上提示|
|Range(0,10)|序列化为滑块|
|RequireComponent(typeof())|依赖组件|
|HelpURL("url")|帮助链接|
|Multiline|string类型添加多行输入|

# 创建自定义资源（.asset）
```csharp
// fileName 当前继承了ScriptableObject脚本的名称
// 在project面板右击`Create\`目录下的名称
[CreateAssetMenu(fileName = "TextStyle", menuName = "New TextStyle", order = 1)]
```