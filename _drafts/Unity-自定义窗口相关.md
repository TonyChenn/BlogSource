---
title: Unity-自定义窗口相关
date: 2020-07-15 09:29:00
updated: 
tags:
img: 
description:
top: 
---
# Button
```csharp
if (GUILayout.Button("保存"))
{
    ///TODO ButtonClick
}
```

# Toggle
```csharp
bool checkd = false;
checkd = GUILayout.Toggle(checkd, "tip");
```
# ObjectField
```csharp
//Transform,AudioClip,etc...
GameObject go;
go = EditorGUILayout.ObjectField(go, typeof(GameObject), true) as GameObject;
```

# ScrollView
```csharp
Vector2 scrollPos;
scrollPos = GUILayout.BeginScrollView(scrollPos);
///all kind of copoments
EditorGUILayout.EndScrollView();
```

# 提示框
```csharp
EditorUtility.DisplayDialog("title", "msg", "btn1"...);
```
