---
title: CSharp反射之方法(4)
date: 2021/2/5 18:23
tags:
top:
password:
description:
img:
---
# 前置类
```csharp
public class Calculate
{
    public Calculate() { Log("调用了无参构造方法"); }
    public Calculate(int a) { Log("带参构造方法：" + a); }

    public void Print() { Log("调用了无参方法"); }
    public void Print(string info) { Log("带参方法：" + info); }

    public static int GetAddValue() { return 0; }
    public static int GetAddValue(int num1, int num2) { return num1 + num2; }

    public void Exchange(ref int a, ref int b)
    {
        int temp = a;
        a = b;
        b = temp;
    }

    void Log(string msg) { Console.WriteLine(msg); }
}
```

# 调用构造方法(有参，无参)
```csharp
//获取并调用无参构造方法
var nonConstr = calc.GetConstructor(Type.EmptyTypes);
nonConstr.Invoke(new object[] { });

//获取并调用有参构造方法
var oneConstr = calc.GetConstructor(new Type[] { Type.GetType("System.Int32") });
if (oneConstr != null)
    oneConstr.Invoke(new object[] { 100 });

//> 调用了无参构造方法
//> 带参构造方法：100
```

# 调用静态方法(有参，无参)
```csharp
//静态无参带返回值方法
var nonStaticMethod = calc.GetMethod("GetAddValue", Type.EmptyTypes);
object o1 = nonStaticMethod.Invoke(null, new object[] { });
Console.WriteLine(o1);
//静态有参带返回值方法
var type = Type.GetType("System.Int32");
var twoStaticMethod = calc.GetMethod("GetAddValue",
                        new Type[] {type,type });
object o2 = twoStaticMethod.Invoke(null, new object[] { 100, 99 });
Console.WriteLine(o2);

//> 0
//> 199
```

# 调用非静态方法(有参，无参)
```csharp
//获取并调用无参方法
var nonParamMethod = calc.GetMethod("Print", Type.EmptyTypes);
nonParamMethod.Invoke(instance, new object[] { });
//获取并调用有参构造方法
var stringType = Type.GetType("System.String");
var oneParamMethod = calc.GetMethod("Print", new Type[] { stringType });
oneParamMethod.Invoke(instance, new object[] { "反射输出" });

//> 调用了无参方法
//> 带参方法：反射输出
```

# 调用含有ref参数的方法
```csharp
Type intType = Type.GetType("System.Int32");
var refMethod = calc.GetMethod("Exchange");
object[] objs = new object[2] { 100, 999 };
refMethod.Invoke(instance, objs);
Console.WriteLine(objs[0] + " : " + objs[1]);

//> 999 : 100
```
