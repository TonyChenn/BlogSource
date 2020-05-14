---
title: CSharp-Json的序列化与反序列化
date: 2018-09-07 18:13:40
tags: 
      - Json
      - LitJson
      - CSharp
      - fastJson
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/icon.jpg
---

## JSON介绍
Json是一种轻量数据交互格式，易于阅读，编写，基于ECMAScript 的一个子集,是一种独立语言。为了数据传输方便，传输时，将要传输的对象序列化为Json，效率极高。接收时，通过反序列化转化成对象。与XML语言效果一样。两者可以相互代替。但比XML更简洁，更容易阅读，传输效率更高。

## 语法规则
- 数据保存在键值对中
- 数据由逗号分隔
- 花括号保存对象
  ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/object.png)
- 方括号保存数组
  ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/array.png)
- 值可以是双引号括起来的字符串、数值(number)、true、false、 null、对象或者数组（array）
   ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/value.png)

## 1. 使用LitJson进行序列化与反序列化

### 序列化

```csharp
public class SkillBean
{
    public int id { get; set; }
    public string name { get; set; }
    public int damage { get; set; }
}
```

```csharp
var SkillList = new List<SkillBean>();
SkillBean skill1 = new SkillBean { id = 101, name = "落叶飞花", damage = 500 };
SkillBean skill2 = new SkillBean { id = 102, name = "暗度陈仓", damage = 304 };
SkillBean skill3 = new SkillBean { id = 103, name = "万丈光芒", damage = 302 };
SkillBean skill4 = new SkillBean { id = 104, name = "十方皆杀", damage = 405 };
SkillBean skill5 = new SkillBean { id = 105, name = "银光落刃", damage = 220 };
SkillBean skill6 = new SkillBean { id = 106, name = "幻影剑舞", damage = 690 };
SkillBean skill7 = new SkillBean { id = 107, name = "狂龙紫电", damage = 666 };

SkillList.Add(skill1);
SkillList.Add(skill2);
SkillList.Add(skill3);
SkillList.Add(skill4);
SkillList.Add(skill5);
SkillList.Add(skill6);
SkillList.Add(skill7);

string json = JsonMapper.ToJson(SkillList);
Console.WriteLine(json);
```
输出结果(汉字自动转化为Unicode)：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/result.jpg)

### 反序列化
- 类型一：

```json
[
  {"id":1,"name": "天下无双","damage": 100},
  {"id":2,"name": "落叶飞花","damage": 200},
  {"id":3,"name": "咫尺天涯","damage": 300}
]
```

```csharp
class SkillBean
{
    public int id { get; set; }
    public string name { get; set; }
    public int damage { get; set; }
}
```

```csharp
//解析
//获取Json字符串
string json = File.ReadAllText("SkillInfo.json");

List<SkillBean> skillList = new List<SkillBean>();
//解析得到skill对象集合
JsonData jsonDataList = JsonMapper.ToObject(json);
foreach (JsonData item in jsonDataList)
{
    SkillBean skill = new SkillBean();
    skill.id = Int32.Parse(item["id"].ToString());
    skill.name = item["name"].ToString();
    skill.damage = Int32.Parse(item["damage"].ToString());
    skillList.Add(skill);
}
```


- 类型二：
```json
{
  "SkillList": [
    { "id": 1, "name": "天下无双", "damage": 100 },
    { "id": 2, "name": "落叶飞花", "damage": 200 },
    { "id": 3, "name": "咫尺天涯", "damage": 300 }
  ]
}
```

```csharp
public class SkillBeanList
{
    public SkillBean[] SkillList { get; set; }
}

public class SkillBean
{
    public int id { get; set; }
    public string name { get; set; }
    public int damage { get; set; }
}
```

```csharp
string json = File.ReadAllText("SkillInfo.json");
var list = JsonMapper.ToObject<SkillBeanList>(json).SkillList;
```


## 2. 使用FastJson进行序列化与反序列化

fastJson序列化，反序列化，都只需要一行代码。且运行速度快.
### 序列化

```csharp
string json = fastJSON.JSON.ToJSON(SkillList);
```

### 反序列化
JsonBean
```csharp
public class Rootobject
{
    public SkillBean[] SkillList { get; set; }
}

public class SkillBean
{
    public int id { get; set; }
    public string name { get; set; }
    public int damage { get; set; }
}
```
```csharp
string json = File.ReadAllText("skillInfo.json");

var Jobject = fastJSON.JSON.ToObject<Rootobject>(json).SkillList;
foreach (var item in Jobject)
{
    Console.WriteLine(item.id + " " + item.name + " " + item.damage);
}
```


## 参考
- [Json官网](https://json.org)
- [LitJson官网](https://litjson.net)
- [fastJson Github](https://github.com/alibaba/fastjson)