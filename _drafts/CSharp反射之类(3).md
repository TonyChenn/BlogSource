---
title: CSharp反射之类(3)
date: 2021-02-05 16:01:00
tags: CSharp
top:
password:
description:
img:
---
前置类
```csharp
public class Animal
{
    public int ID = 1001;
    string mName;
    int mAge;

    public int Age{get{ return mAge; }set { mAge = value; } }
    public string Name { get { return mName; } set { mName = value; } }

    public Animal() { }
    public Animal(string name) { this.mName = name; }
    public Animal(string name, int age) { this.mName = name; this.mAge = age; }
    public void Speak(string msg) { Console.WriteLine(mName + " says: " + msg); }
}

class Cat:Animal
{
    public Cat(string name, int age) : base(name, age) { }
    public void CatchMouse() { }
    void EatFish() { }
}
```
# 判断是类
```csharp
Cat tom = new Cat("Tom", 11);
Type type = tom.GetType();
Console.WriteLine("IsClass: "+type.IsClass);
//> True
```

# 获取基类
```csharp
Console.WriteLine("BaseClass: " + type.BaseType);
//> BaseClass: Demo.Animal
```
# 类的属性
## 获取单个属性
```csharp
//获取属性
var ageProperty = type.GetProperty("Age");
```
## 获取所有属性
```csharp
//获取所有属性
PropertyInfo[] properties = type.GetProperties();
foreach (var item in properties)
    Console.WriteLine(item.Name);
//> Age
//> Name
```
## 获取，修改属性值
```csharp
Console.WriteLine("before Tom is " + ageProperty.GetValue(tom) + " years old");
ageProperty.SetValue(tom, 100);
Console.WriteLine("after Tom is " + ageProperty.GetValue(tom) + " years old");

//> before Tom is 11 years old
//> after Tom is 100 years old
```

# 类的字段(只能是public类型)
```csharp
Console.WriteLine("所有字段如下：");
FieldInfo idInfo = type.GetField("ID");
Console.WriteLine(idInfo.GetValue(tom));

FieldInfo[] fields = type.GetFields();
foreach (var item in fields)
{
    Console.WriteLine(item.Name);
}
```

# 类的成员
什么是成员？？和字段什么区别？