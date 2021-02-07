---
title: CSharp反射之接口(2)
date: 2021-02-05 11:46:00
tags: CSharp
top:
password:
description:
img:
---
# 判断是不是接口
```csharp
int a = 100;
Console.WriteLine(num.GetType().IsInterface.ToString());
//>false
```
# 获取接口
```csharp
List<int> list = new List<int>();
Type type = list.GetType();
//获取所有接口
var interfaces = type.GetInterfaces();
//获取某个接口
var ilist = type.GetInterface("IList")
```

# 反射获取继承某个接口的类
```csharp
//获取所有继承 IEditorPrefs 的类
Assembly assembly = Assembly.Load("Assembly-CSharp-Editor");
System.Type[] types = assembly.GetTypes();
for (int i = 0, iMax = types.Length; i < iMax; i++)
{
    //如果是接口
    if (types[i].IsInterface) continue;

    // 获取继承的所有接口
    System.Type[] ins = types[i].GetInterfaces();
    foreach (var item in ins)
    {
        if (item == typeof(IEditorPrefs))
        {
            //TODO
            //调用接口中的 ReleaseEditorPrefs 方法
            object o = System.Activator.CreateInstance(types[i]);
            MethodInfo method = item.GetMethod("ReleaseEditorPrefs");
            method.Invoke(o, null);
            break;
        }
    }
}
```