---
title: xLua笔记之LuaCallCSharp(2)
date: 2020-10-28 18:53:00
tags:
    - Unity
    - Lua
    - xLua
    - 热更新
top:
password:
description:
img:
---

# C#访问全局属性
```lua
-- lua代码
name = "TonyChenn"
url ="tonychenn.cn"
age = 23
```
```csharp
//访问字段
string name = luaEnv.Global.Get<string>("name");
int age = luaEnv.Global.Get<int>("age");
```
# C#访问全局table
通过类,结构体，接口映射时，<b>类，结构体，接口中的字段，方法名应与lua中的字段，方法名保持一致</b>。

## 通过类和结构体映射
1. 通过类和结构体是将lua中数据拷贝一份，修改后的字段值不会同步到table，反过来也不会。
2. 暂时没找到通过类（结构体）映射lua中方法，可通过下面接口的方式映射（有大佬知道的，下面留言告知下，谢谢）
3. 映射类不需要添加"CsharpCallLua"特性
4. 开销大，官方不建议使用

```lua
animal={name="Panda", age = 1}
function animal:eat(food){
    print("吃"..food)
}
```
```csharp
//执行lua表
luaEnv.DoString("require 'test'");

//访问表
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");
Animal animal = luaEnv.Global.Get<Animal>("animal");
print("修改前" + animal.name + "\t" + animal.age);
animal.age = 99;
animal.name = "tortoise";
print("修改后" + animal.name + "\t" + animal.age);
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");

public class Animal
{
    public string name;
    public int age;
}
```
输出结果如下：
```log
LUA: lua print: Panda : 1
修改前Panda	1
修改后tortoise	99
LUA: lua print: Panda : 1
```

## 通过接口映射
1. 通过接口会直接修改lua中数据
2. 需要添加“CSharpCallLua”特性，依赖xlua生成代码（如果没生成代码会抛<b>InvalidCastException</b>异常）
```csharp
// lua代码与上面一样

//下面是通过接口访问属性和方法
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");
IAnimal animal = luaEnv.Global.Get<IAnimal>("animal");
print("修改前" + animal.name + "\t" + animal.age);
animal.eat("bamboo");
animal.age = 99;
animal.name = "tortoise";
print("修改后" + animal.name + "\t" + animal.age);
luaEnv.DoString("print('lua print: '..animal.name..' : '..animal.age)");

[CSharpCallLua]
public interface IAnimal
{
    string name { get; set; }
    int age { get; set; }
    void eat(string food);
}
```
输出结果
```
LUA: lua print: Panda : 1
修改前Panda	1
LUA: 吃bamboo
修改后tortoise	99
LUA: lua print: tortoise : 99
```

## 映射到Dictionary,List
```
```
## 使用by ref方式：映射到LuaTable类
不需要生成代码，但是比较慢，比映射到interface慢一个数量级，没有类型检查
```csharp
LuaTable table = luaEnv.Global.Get<LuaTable>("animal");
string name = table.Get<string>("name");
Debug.Log(name);

//博主没找到在LuaTable中调用表中方法，尝试下面方法，会报“attempt to concatenate a nil value (local 'food')”就是拿到委托后执行传递的apple,在lua中没有接收到。待解决,有知道原因和解决方法的欢迎留言。
//Action<string> action = table.Get<Action<string>>("eat");
//action("apple");

//action = null;
```


# C#访问全局方法
## 映射到Action，delegate
官方建议，性能好很多，类型安全，但是要生成代码。注意Action委托没有返回值，有返回值使用delegate，lua多个返回值可以使用C#的ref,out接收。
```lua
function eat(food)
    print("喜欢吃"..food)
end

function add(a,b)
    return a+b
end
function test(a,b,c)
    return a+b+c, a+b, a+c, b+c
end
```
```csharp
[CSharpCallLua]
public delegate int AddDelegate(int a, int b);

int a_b, a_c, b_c;  //a+b, a+c, b+c
[CSharpCallLua]
public delegate int TestDelegate(int a, int b, int c, out int a_b, out int a_c, out int b_c);

//...

//action委托（不能包含返回值）
Action<string> action1 = luaEnv.Global.Get<Action<string>>("eat");
action1("apple");

//delegate委托，一个返回值
AddDelegate add = luaEnv.Global.Get<AddDelegate>("add");
int sum = add(100, 200);
Debug.Log(sum);

//delegate多个返回值
TestDelegate test = luaEnv.Global.Get<TestDelegate>("test");
int _sum = test(100, 200, 500, out a_b, out a_c, out b_c);
Debug.Log(string.Format("a+b+c={0} a+b={1} a+c={2} b+c={3}", _sum, a_b, a_c, b_c));

action1 = null;
add = null;
test = null;

//...

luaEnv.Dispose();
```
输出结果如下：
```
LUA: 喜欢吃apple
300
a+b+c=800 a+b=300 a+c=600 b+c=700
```


## 映射到LuaFunction
优缺点刚好和第一种相反。
```csharp
//一个参数
LuaFunction OneParam = luaEnv.Global.Get<LuaFunction>("eat");
OneParam.Call("Orange");
//一个返回值
LuaFunction ParamWithReturn = luaEnv.Global.Get<LuaFunction>("add");
object[] sum = ParamWithReturn.Call(100, 99);
Debug.Log(sum[0]);
//多个返回值
LuaFunction MulitReturns = luaEnv.Global.Get<LuaFunction>("test");
object[] results = MulitReturns.Call(100, 200, 500);
Debug.Log(string.Format("a+b+c={0} a+b={1} a+c={2} b+c={3}", results[0], results[1], results[2], results[3]));
```