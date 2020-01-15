---
title: Unity-编辑器拓展
date: 2019-11-19 09:28:16
tags: Unity
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/icon.png
description:
top:
---
对于编辑器拓展代码通常情况下会存放在工程根目录下的"Editor/"目录中，此目录下代码只会在引擎中调用运行，打包时**不会**打包进工程

# 创建菜单

## 顶部栏按钮
1. 引用命名空间
2. 给方法添加标记
3. 方法必须为静态方法

```csharp
using UnityEditor;

public class ExTool
{
    [MenuItem("Tools/拓展工具")]
    static void MyItemMenu()
    {
        UnityEngine.Debug.Log("测试");
    }
}
```
## 菜单方法显示优先级
通常情况下自上而下排序，优先级默认都为1000，更改优先级就可进行排序
```csharp
using UnityEditor;
using UnityEngine;

public class ExTool
{
    [MenuItem("Tools/拓展工具1",false,3)]
    static void MyItemMenu1()
    {   Debug.Log("测试1");   }
    [MenuItem("Tools/拓展工具2", false,2)]
    static void MyItemMenu2()
    {   Debug.Log("测试2");   }
    [MenuItem("Tools/拓展工具3", false,1)]
    static void MyItemMenu3()
    {   Debug.Log("测试3");   }
}
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/unsort.png)

## Hierarchy窗口鼠标右击菜单
即将自定义菜单路径设置在"GameObject/"路径下;
根据需要将优先级设置为1-10;
```csharp
using UnityEditor;
using UnityEngine;

public class ExTool
{
    [MenuItem("GameObject/拓展工具1",false,3)]
    static void MyItemMenu1()
    {
        Debug.Log("测试1");
    }
}
```

## Project窗口右击菜单
只需要将路径设置在"Assets/"路径下即可

## Inspector窗户右击菜单(MenuItem方式)
1. 需要指定需要拓展所对应的组件
2. 位于"CONTEXT/"路径下

```csharp
public class ExTool
{
    [MenuItem("CONTEXT/Camera/工具1")]
    static void MyItemMenu1()
    {
        Debug.Log("测试1");
    }
}
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/Inspertor.jpg)
## 获取当前操作对象
```csharp
[MenuItem("CONTEXT/Camera/绿色背景")]
static void MyItemMenu1(MenuCommand cmd)
{
    Camera camera = cmd.context as Camera;
    camera.backgroundColor = Color.green;
}
```

# 物体操作
## 获取选中的物体(一个)
1. 如果选择多个，则只返回第一个
```csharp
[MenuItem("MyTool/LogSelectObjName")]
static void LogSelectObjName()
{
    var name = Selection.activeGameObject.name;
    Debug.Log(name);
}
```
## 获取多个选中的物体
```csharp
[MenuItem("MyTool/LogSelectAllObjName")]
static void LogSelectAllObjName()
{
    var objs = Selection.gameObjects;
    foreach (var item in objs)
        Debug.Log(item.name);
}
```
## 按钮的启用与禁用
当上面方法没有选中物体就输出信息的话会出错，所以我们需要当没有选中物体时就禁用该按钮。
```csharp
[MenuItem("MyTool/LogSelectObjName %l", true)]
static bool HaveSelecetedObj()
{
    if(Selection.gameObjects.Length > 0)
        return true;
    else
    {
        Debug.Log("还没有选择物体");
        return false;
    }
}
[MenuItem("MyTool/LogSelectObjName %l",false)]
static void LogSelectObjName()
{
    var name = Selection.activeGameObject.name;
    Debug.Log(name);
}
```
# 添加快捷键
- 组合键：路径后面+空格+按键
- 单个按键：路径后面+空格+"_"+按键

```csharp
//% 代表Ctrl
//# 代表Shift
//& 代表Alt

[MenuItem("MyTool/LogSelectObjName %l")]
[MenuItem("MyTool/LogSelectObjName _l")]
```

# ContextMenu
## Inspector窗口右击菜单(ContextMenu方式)
1. 位于UnityEngine命名空间下，需要继承Monobehavior
2. 不能放在Editor目录下
3. 对自定义脚本方法的拓展
```csharp
public class InspectorEx : MonoBehaviour
{
    [ContextMenu("拓展菜单")]
    void ButtonEx()
    {
        Debug.Log("Tool");
    }
}
```

# ContextMenuItem
1. 位于UnityEngine命名空间下，需要继承Monobehavior
2. 不能放在Editor目录下
3. 对自定义脚本属性的拓展
```csharp
[ContextMenuItem("Add count","Add")]
public int Count = 100;

void Add()
{
    this.Count += 10;
}
```

# ScriptableWizard对话框
```csharp
public class Dialog :ScriptableWizard
{
    public string value = "1";
    [MenuItem("MyTool/CreateWnd")]
    static void ShowWnd()
    {
        DisplayWizard<Dialog>("title","CreateButton","OtherButton");
    }
    OnEnable(){ }
    // Create按钮方法
    void OnWizardCreate()
    {
        Debug.Log("Create method");
    }
    // OtherButtonClick
    void OnWizardOtherButton()
    {
        helpString = "Click OtherButton";
    }
    // 创建对话框时调用一次
    // 对话框内值更改时调用
    void OnWizardUpdate()
    {
        if (Selection.gameObjects.Length == 0)
            errorString = "至少选择一个物体";
        else
            helpString = "选择了" + Selection.gameObjects.Length + "个物体";
    }
    // 当选择物体发生改变时调用
    void OnSelectionChange()
    {
        errorString = null;
        helpString = null;
        OnWizardUpdate();
    }
}
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/ScriptableWizard.jpg)
# 数据保存EditorPrefs
与PlayerPrefs相对，在编辑器模式下有EditorPrefs，用法一致。

# 显示进度条EditorUtility

# 自定义窗口
- 自定义窗口的界面使用OnGUI绘制
- OnGUI的笔记会有一篇专门文章

```csharp
public class MyWindow : EditorWindow 
{
    [MenuItem("MyTool/New Window")]
    static void ShowWindow()
    {
        MyWindow window = EditorWindow.GetWindow<MyWindow>();
        window.Show();
    }

    string name;
    void OnGUI()
    {
        GUILayout.Label("新窗口");
        name = GUILayout.TextField(name);
        if(GUILayout.Button("创建"))
        {
            GameObject obj = new GameObject(name);
            //添加可撤销操作
            Undo.RegisterCreatedObjectUndo(obj, "Create obj");
        }
    }
}
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/1119/MyWindow.jpg)

