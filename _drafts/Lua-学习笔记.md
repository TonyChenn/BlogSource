---
title: Lua-学习笔记
date: 2020-6-17 14:33:23
updated: 
tags:
img: 
description:
top: 
---
# 各种循环
## while循环
```lua
local a=10
while a>0 do
    print(a);
    a=a-1;
end
```

## for循环
```lua
-- 遍历数字
for i=1,10 do
    print(i);
end

-- 迭代数组列表
for k,v in ipairs(table) do 
    ...
end

-- 迭代hash表
for k,v in pairs(table) do
    ...
end
```

# 流程控制
```lua
-- 判断最大值
if a>b then
    print(a);
else
    print(b);
end

-- 成绩档次
if score==100 then
    print("A");
elif score>=80 and score<100 then
    print("B");
else
    print("C");
end
```

# 函数
lua函数可以返回<kbd>多个值</kbd>

## 返回多个值
遍历一个列表，返回列表中的两个最大值，代码如下：
```lua
local list={1,5,2,7,3,8};
local function twoMax(list)
    local first_max=0;
    local second_max=0;
    for k,v in pairs(list) do
        if v > first_max then
            second_max = first_max;
            first_max = v;
        end
    end
    return first_max,second_max;
end
print(twoMax(list));
```

## 可变参数
使用可变参数必须在固定参数后面，求平均值代码如下：
```lua
local function average(...)
    local result=0;
    local args={...};   -- 使用表承接所有的参数
    for k,v in pairs(args) do
        result=result+v;
    end
    return result/ #args;
end
```

# 运算符

## 数学运算符
|符号|描述|
|---|---|
|+|加法|
|-|减法|
|*|乘法|
|/|除法|
|%|取余|
|^|乘幂|

## 关系运算符
|符号|描述|
|---|---|
|==|相等|
|~=|不相等|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|

## 逻辑运算符
|符号|描述|
|---|---|
|and|且|
|or|或|
|not|非|

## 其他运算符
|符号|描述|
|---|---|
|..|连接字符串|
|#|返回字符串或表的长度|

# 字符串操作
|方法|描述|
|---|---|
|upper(str)|全转大写|
|lower(str)|全转小写|
|gsub(origin,sub,time)|在origin中替换sub,time次|
|find(origin,sub,index)|返回第inex个sub字符串在origin中的开始结束索引，不存在返回nil|
|reverse(string)|字符串反转|
|format()|字符串格式化（与C语言相似）|
|len(string)|取长度|
|rep(str,n)|返回str的n个拷贝|

# Lua table(表)
1. table.concat,将表中数据连接成字符串
2. table.insert(a_table,value,pos),将数据插到pos位置，pos默认为table末尾
3. table.remove(a_table,pos),从a_table中删除pos位置的数据，pos默认为table的末尾
4. table.sort(table)

# 定义包与引用包

- 定义包
```lua
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module
```
- 引用包
```lua
require("module_name")

require "module_name"
```

# 协程

# IO

# 面向对象


# pairs与ipairs的区别
1. ipairs(table) 再遍历过程中遇到值为nil时就停止迭代,不能返回nil值。
2. pairs(table) 再遍历过程中遇到nil值可以继续迭代，可以返回nil值。