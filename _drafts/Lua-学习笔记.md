---
title: Lua-学习笔记
date: 2020-06-17 14:33:23
updated: 2020-10-26
tags:
    - Lua
    - 热更新
img: 
description:
top: 
---

# 变量
1. lua的布尔类型只有nil和false时false,其他都是true.
2. lua中没有加local修饰的变量都是全局变量。方法中没加local也是全局变量。

# 各种循环
1. lua中不等于是："~="
2. lua中字符串拼接是："str_a..str_b",加号是计算两个数字现加。

## while循环
```lua
while condition do print("") end
```
## for循环
```lua
-- 遍历数字
for i=1,10 do ... end

-- 迭代数组列表，ipair在遍历过程中遇到值为nil时就停止迭代,不能返回nil值
for k,v in ipairs(table) do ... end

-- 迭代hash表，遍历过程中遇到nil值可以继续迭代，可以返回nil值
for k,v in pairs(table) do ... end
```
## until循环
```lua
sum = 2
repeat ... until condition
```
# 函数
样子和javascript挺像,允许多个返回值。也是前面加上<kbd>local</kbd>变成局部函数。

## 返回多个值
```lua
function fun0() end
function fun1() return "a" end
function fun2() return "a","b" end
```
在多重赋值时，如果方法是放在表达式最后面，则该函数会尽可能多的返回值来匹配待赋值的变量。
```lua
x, y = fun0()        -- x:nil, y:nil
x, y = fun1()        -- x:"a", y:nil
x, y, z = fun2()     -- x:"a", y:"b", z=nil
t={20, fun2()}       -- t[1]:20, t[2]:"a", t[3]:"b"
```
如果没有放在表达式最后面，则表达式只会返回1个值
```lua
x, y = fun2(), 20       -- x:"a", y:20
x, y = fun2(), 20, 30   -- x:"a", y:20
t = {fun0(), fun2(), 30}-- t[1]=nil, t[2]="a", t[3]=30
```

## 可变参数
三个点表示可变参数，使用可变参数必须在固定参数后面，求平均值代码如下：
```lua
local function average(...)
    local result=0
    for k,v in pairs({...}) do
        result=result+v
    end
    return result/ #args
end
```
遍历参数不存在nil的可变参数时可以按照上面的方法通过表承接的方式拿到所有参数`{...}`是有效的；但是当参数中存在无效的nil时，那么`{...}`也会变得无效，此时可以通过`table.pack`的方式将所有的参数放到一张表中，同时存储字段n来保存参数的长度。
```lua
local result = table.pack(...)
local len = result.n
```
遍历可变参数的另一种方式是使用`select函数`:
```lua
print(select (1,"a","b","c"))    -- > a b c 
print(select (2,"a","b","c"))    -- > b c
print(select (3,"a","b","c"))    -- > c
print(select ("#","a","b","c"))  -- > 3
```

# Lua table(表)
- lua的table是一个key-value的数据结构。只是key的数据类型可以不相同，值的数据类型也可以不相同，也可以是方法。
- lua中表中第一个值的索引时1，从1开始，与其他语言不相同。
- 可以通过<kbd>#animal</kbd>获取表的长度。
```lua
animal={name="panda", age=6}
```
- 在lua中，也是用table管理没有使用local修饰的全局变量，存放在一个叫<kbd>_G</kbd>的表中.可以通过下面方法访问全局变量.
```lua
print(_G.animal)
print(_G["animal"])
```
## table.pack
参看上面 "可变参数"
## table.unpack
上面用到`table.pack`用来将可变参数转换成一个表，同样`table.unpack`的作用就是将一张表中数据取出成一个个参数。如果有需要，也可以控制返回范围
```lua
a, b = table.unpack({"A", "B", "C"})        -- a:"A", b:"B"
-- 返回从2到3的数据
a, b = table.unpack({"A", "B", "C"}, 2, 3)  -- a:"B", b:"C"
```

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
## string.find
## string.match
返回匹配的字符串，可用作正则匹配。
## string.gsub
```lua
-- string.gsub(str_origin, sub, replace, times)
print(string.gsub("Hello, world!", "l", "Z", 2))
--输出：HeZZo, world!

-- 其中第三个参数也可以还可以是函数
--string.gsub(str_origin, sub, function, times)
function show(str) print("->"..str) end
string.gsub("Hello, world!","l",show,1)
-- 输出：->l
```

# lua 正则（模式匹配）
|符号|解释|
|---|---|
|.|任意字符|
|%a|字母|
|%c|控制字符|
|%d|数字|
|%g|除空格外的可打印字符|
|%l|小写字母|
|%p|标点符号|
|%s|空白字符|
|%u|大写字母|
|%w|字母+数字|
|%x|16进制|

```lua
print(string.gsub("Hello, world!","%A","*"))
-- 输出： Hello**world*
```

修饰符：
|符号|解释|
|---|---|
|+|重复1次或多次|
|*|重复0次或多次|
|-|重复0次或多次(最小匹配)|
|?|出现0次或1次|

# 时间
```lua
-- 获取时间戳
local time = os.time()
-- 获取日期
local date = os.date("*t", time)
```
格式化输出：



# MetaTable与MetaMethod
MetaTable是用来做一些重写运算符的操作。例如a和b同学一块统计班级里学生信息，得到a={boy=13,girl=15},b={boy=18,girl=16}，现在要合并数据a+b操作时肯定不行的，这是就可以通过MetaTable实现a+b的计算:
```lua
calculate={}
function calculate.__add(a,b)
    local temp = {}
    temp.boy = a.boy + b.boy
    temp.girl = a.girl + b.girl
    return temp
end

-- 为a, b设置MetaTable
setmetatable(a, calculate)
setmetatable(b, calculate)
```
这样就可以直接通过a+b得到结果了。

其他的metaMethod:
|MetaMethod|表达式|
|---|---|
|__add(a, b)|a + b|
|__sub(a, b)|a - b|
|__mul(a, b)|a * b|
|__div(a, b)|a / b|
|__mod(a, b)|a % b|
|__pow(a, b)|a ^ b|
|__unm(a)|-a|
|__concat(a, b)|a .. b|
|__len(a)|#a|
|__eq(a, b)|a == b|
|__lt(a, b)|a < b|
|__le(a, b)|a <= b|
|__index(a, b)|a.b|
|__newindex(a, b, c)|a.b = c|
|__call(a, ...)|a(...)|

# 定义包与引用包
通过`require("module_name")`或者`require "module_name"`加载别的lua文件。第一次加载的时候会执行一边，如果之后再次加载的时候就不会再次执行了。当然可以通过`dofile('file_name')`的方式让每次加载的时候都运行一次。同样想只是加载不执行，等需要执行的时候再执行可以用`loadfile('file_name')`.

- 定义包
```lua
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module:func1()
    print("这是一个公有函数！\n")
end
 
local function module:func2()
    print("这是一个私有函数！")
end
 
function module:func3()
    self:func2()
end
 
return module
```
- 引用包
```lua
require("module_name")

require "module_name"
```

# 协程
协程与线程（thread）差不多，也是一条执行序列，拥有自己独立的栈、局部变量和指令指针，同时又与其他协同程序共享全局变量和其他大部分东西。
线程与协程的主要区别在于：一个具有多个线程的程序可以同时运行多个线程，而协程需要彼此协作地运行。就是说，<b>一个具有多个协程的程序在任意时刻只能运行一个协同程序，并且正在运行的协同程序只会在其显式地要求挂起(suspend)时，它的执行才会暂停</b>。
# IO

# 面向对象



