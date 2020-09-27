---
title: Unity-ToLua笔记
date: 2020/9/2 14:59
tags:
---
# 套路
```csharp
//创建Lua虚拟机
LuaState lua = new LuaState();

//启动虚拟机
lua.Start();

//执行lua脚本后的检验
lua.CheckTop();

//释放虚拟机
lua.Dispise();
lua = null;
```


# CSharp调用Lua
```lua
-- luaFile.lua

num1 = 123;
table1 = {"Hello", "World", "ToLua"};

function Add(a,b)
    return a+b;
end

function Log(str)
    print("------>" .. str);
end

-- 多返回值
function multiReturn()
    return 1, true, "Hello";
end
```

## 调用lua字符串（DoString）
```csharp
string str_lua = "print('---->Hello world')";
lua.DoString("str_lua",ScriptName); //变量名，脚本名
```

## 调用Lua文件
```csharp
string lua_folder_path="C:/desktop";
//添加搜索路径(只有添加后才能执行下面的方法)
lua.AddSearchPath(lua_path);

//执行lua文件
lua.DoFile("luaFile.lua");
//引用lua文件
lua.Require("luaFile.lua");
```

## 获取Lua中方法
```csharp
LuaFunction luaFunc = lua.GetFunction("Add");
```

## 执行lua方法
```csharp
//1. luaFunc.Invoke
//参数类型+(最后一个为返回类型)
int num = luaFunc.Invoke<int,int,int>(123,345);

//2. lua.Invoke
int num=lua.Invoke("Add", 123, 234, true);

//3. 转化为委托
Func<int, int, int> func = luaFunc.ToDelegate<Func<int, int, int>>();
int num = func(123,234);
```

## Lua多个返回值
```csharp
-- 方法1
LuaFunction func=lua.GetFunction("multiReturn");
func.BeginPCall();
func.Push(array);
func.PCall();
double arg1 = func.CheckNumber();
string arg2 = func.CheckString();
bool arg3 = func.CheckBoolean();

-- 方法2
object[] return_value_objs = func.LazyCall((object)array);
```

## 获取Lua中的变量
```csharp
//获取变量
int num = lua["num1"];
//获取方法
LuaFunction func = lua["Add"] as LuaFunction;
//获取Lua中的表
LuaTable table = lua.GetTable("table1");
```

## Lua中使用协程
```lua
-- 开启协程
coroutine.start(function_name);

-- 结束协程
coroutine.stop(function_name);

-- 等待一帧
coroutine.step();

-- 等待几秒
coroutine.wait(0.1);

-- www加载
coroutine.www(UnityEngine.WWW("http://www.baidu.com"))
```

# Lua调用CSharp

# ToLua解析json
```json
{
    "name":"TonyChenn",
    "url":"tonychenn.cn",
    "tag":["123","234","345"]
}
```
```tolua
local json=require("cjson");
function encode
```

# 字典

# Tips
1. 再Lua中调用CSharp中变量用".",调用方法用":";