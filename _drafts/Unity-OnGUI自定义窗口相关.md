---
title: Unity-OnGUI自定义窗口相关
date: 2020-07-15 09:29:00
updated: 
tags:
    - Unity
img: 
description: OnGUI绘制编辑器开发自定义窗口
top: 
---

# 基本控件
## 按钮(Button)
```csharp
if (GUILayout.Button("保存"))
{
    ///TODO ButtonClick
}
```

## 复选框(Toggle)
```csharp
bool checkd = false;
checkd = GUILayout.Toggle(checkd, "tip");
```
## ObjectField
```csharp
//Transform,AudioClip,etc...
GameObject go;
go = EditorGUILayout.ObjectField(go, typeof(GameObject), true) as GameObject;
```

## ScrollView
```csharp
Vector2 scrollPos;
scrollPos = GUILayout.BeginScrollView(scrollPos);
///all kind of copoments
EditorGUILayout.EndScrollView();
```

## 提示框
```csharp
EditorUtility.DisplayDialog("title", "msg", "btn1"...);
```
