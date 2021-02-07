---
title: CSharp反射之类型判断(1)
date: 2021-2-5 12:00:00
tags: CSharp
top:
password:
description: 
img:
---
# 访问修饰符
## 访问修饰符有：
public, internal, protected, protected internal, privite
## 判断访问修饰符
|判断方法|解释|
|---|---|
|IsPublic|public|
|IsNotPublic|privite, protected,internal|

C#的protected, internal在IL中没有意义，不会用于反射API,但是也是可以通过下面方式判断的。
|判断方法|解释|
|---|---|
|IsAssembly|是不是 internal|
|IsFamily|是不是 protected|
|IsFamilyOrAssembly|是不是 protected internal|

# 值类型
枚举
基础类型(int,float,char)
结构体

# 类
|判断方法|解释|
|---|---|
|IsClass|类或委托(不是值类型或接口)|
|IsGenericType|类或委托是不是泛型类型|
## 继承
父类，接口

## 类的成员
方法
字段
属性
索引器
事件




C# 成员关键字: readonly, const
C# 声明修饰符: sealed, virtual, abstract, override, static, new




# System.Type
表示类型声明有：类、接口、数组、值类型、枚举、类型参数、泛型类型定义，以及开放或封闭构造的泛型。

常用属性有:
```csharp
Name           //类型名
Namespace      //命名空间

//判断类型
IsInterface         //接口(不是类或值类型)

IsAbstract          //抽象类并且必须被重写
IsVirtual           //虚方法
IsHideBySig         //判断是否有抽象(virtual、override、abstract、new 修饰的方法都是 true)


             //
BaseType            //该类的基类

IsValueType         //值类型
IsPrimitive         //基础类型
IsEnum              //枚举
IsArray             //数组
Attributes          //当前类型相关的属性

IsGenericType       //是否是泛型
GenericTypeArguments//获取此类型泛型类型参数的数组


//访问修饰符


//其他修饰符
IsSealed            //

IsStatic            //是否是静态(const 也是静态)
```
常用方法有：
```csharp
GetInterfaces()         //获取继承的所有接口
GetInterface(String)

IsSubclassOf(type)      //是否派生于type

GetMethods()            //所有公共方法
GetMethod(String)
GetParameters()         //获取方法中所有参数


GetFields()             //所有字段
GetProperties()         //所有属性

GetArrayRank()          //获取数组维度
GetElementType()        //回去数组元素类型

GetConstructors()       //获取公共构造函数

GetEnumNames()          //枚举的所有成员名称
GetEnumValues()

GetEvents()             //所有公共事件
GetEvent(String)

GetFields()             //所有公共字段
GetField(String)
```

获取Type
```csharp
//获取变量类型
typeof(a)
a.GetType()

//从程序集获取所有Type:
System.Type[] types = assembly.GetTypes();
```
