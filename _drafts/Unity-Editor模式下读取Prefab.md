---
title: Unity-Editor模式下读取Prefab
date: 2020-07-17 16:22:00
updated: 
tags: Unity
img: 
description:
top: 
---

在编辑器模式下加载Prefab，并进行读取操作。

```csharp
//返回匹配到资源的GUID 数组
string[] guids = AssetDatabase.FindAssets("t:Prefab", searchFolderArray);

foreach (var guid in guids)
{
    //获取每个GUID的预设地址
    string prefabPath = AssetDatabase.GUIDToAssetPath(guid);
    //根据路径加载预设
    var obj = AssetDatabase.LoadAssetAtPath<GameObject>(prefabPath);
    //读取预设所有子节点身上的组件
    //参数是是否包含隐藏的节点
    UILabel[] labels = obj.GetComponentsInChildren<UILabel>(true);
    foreach(var item in labels)
    {
        ///TODO
    }
}

...
AssetDatabase.Refresh();
```