---
title: Unity编辑器拓展之Inspector
date: 2021-2-27 15:13:00
tags:
    - Unity
    - UnityEditor
top:
password:
description: Inspector面板的各种控件
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/icon.png
---
通过将字段声明为`public`类型即可在`Inspector`面板中看到，如果不想将public类型变量显示在Inspector，可以使用`HideInInspector`。也可以使用`SerializeField`将私有类型的变量显示在Inspector面板。

# 滑块进行范围选择
```csharp
[Range(0,100)]
public int Age;
[Range(0,10)]
public float Hp;
```
![range](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/range.jpg)

# 文本输入框修饰
默认的文本输入框是单行的TextArea,可以通过`TextArea,Multiline`来实现多行输入，个人感觉`Multiline`比`TextArea`功能更强大。区别如下：
1. 前者不会自动换行，后者可以自动换行显示
2. 前者不能通过鼠标滑轮滚动，后者可以
3. 前者输入多行没有滚动条，后者有。
```csharp
[Multiline(4)]
public string Content;
[TextArea(4,6)]
public string Info;
```
![textarea](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/textarea.jpg)

# 颜色选择框
默认情况与使用`ColorUsage(true)`相同，参数是是否显示Alpha通道。
```csharp
[ColorUsage(false)]
public Color color1;
public Color color2;
```
# 属性间空格
试用`Space`进行对该属性的向下偏移
```csharp
[Space(15)]
public string Name;
```
# 提示信息
```csharp
[Tooltip("这是一个提示")]
public string Name;
```
![tooltip](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/tooltip.jpg)

# 组件的右击菜单
直接放在方法上面。
```csharp
[ContextMenu("按钮1")]
void ContextMenuHandler()
{
    Debug.Log("测试");
}
```
![contextmenu](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/contextmenu.jpg)

# 单个属性右击菜单
```csharp
[ContextMenuItem("Reset", "ResetFunction")]
[ContextMenuItem("Random", "RandomFunction")]
[Range(1, 100)]
public int HP;

void ResetFunction() { this.HP = 100; }
void RandomFunction() { this.HP = Random.Range(0, 100); }
```
![contextmenuitem](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2021/0227/contextmenuitem.jpg)