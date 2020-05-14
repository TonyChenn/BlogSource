---
title: CSharp-XML解析
date: 2018-09-07 12:37:02
tags:
     - CSharp
     - XML
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.07/xml.jpg
---

可扩展标记语言，标准通用标记语言的子集，是一种用于标记电子文件使其具有结构性的标记语言。
在电子计算机中，标记指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种的信息比如文章等。它可以用来标记数据、定义数据类型，是一种允许用户对自己的标记语言进行定义的源语言。 它非常适合万维网传输，提供统一的方法来描述和交换独立于应用程序或供应商的结构化数据。是Internet环境中跨平台的、依赖于内容的技术，也是当今处理分布式结构信息的有效工具。早在1998年，W3C就发布了XML1.0规范，使用它来简化Internet的文档信息传输。

## 前言
- XML与HTML的区别

XML | HTML
---|---
被设计用来传输存储数据 | 旨在显示数据

（敲黑板）两者不是同样的语言(画重点)

 ## Parse
 
```csharp
//from file,url
var XmlDoc = new XmlDocument();
XmlDoc.Load(filePath);
XmlDoc.Load(xmlUrl);

//from xmlString
XmlDoc.LoadXml(str_xml);
```

## SelectNode(s)

```csharp
//1.Select Nodes
var skillNodeList = rootNode.ChildNodes;
var skillNodeList = rootNode.SelectNodes(Xpath);

//2.Select Node
var skillNodeList = rootNode.SelectSingleNode(XPath);
```

Xml的解析过程与Html类似，可以参考之前的文章[CSharp-使用HtmlAgilityPack实现网站爬虫总结](https://tonychenn.cn/2018/09/01/CSharp-%E4%BD%BF%E7%94%A8HtmlAgilityPack%E5%AE%9E%E7%8E%B0%E7%BD%91%E7%AB%99%E7%88%AC%E8%99%AB%E6%80%BB%E7%BB%93/)

## 小例子

SkillInfo.xml
```xml
<skills>
  <skill id="1">
    <name>天下无双</name>
    <damage>100</damage>
  </skill>
  <skill id="2">
    <name>落叶飞花</name>
    <damage>200</damage>
  </skill>
  <skill id="3">
    <name>咫尺天涯</name>
    <damage>300</damage>
  </skill>
</skills>
```

SkillBean.cs 对应Xml文档中的属性

```csharp
class SkillBean
{
    public int id { get; set; }
    public string name { get; set; }
    public string damage { get; set; }
}
```
将SkillInfo中 Skill对应Id,name,damage，解析出来

```csharp
static void Main(string[] args)
{
    //定义一个列表用来存放解析出来的数据
    var SkillList = new List<SkillBean>();
    var XmlDoc = new XmlDocument();
    //加载XML文件
    XmlDoc.Load("SkillInfo.xml");

    //获取root节点
    var rootNode = XmlDoc.FirstChild;
    //获取root节点下的所有子节点，（即Skill节点）
    var skillNodeList = rootNode.ChildNodes;
    //遍历所有skill节点
    foreach (XmlNode skillNode in skillNodeList)
    {
        SkillBean skill = new SkillBean();

        skill.id= Int32.Parse(skillNode.Attributes["id"].Value);

        var SkillPropertyList = skillNode.ChildNodes;
        foreach (XmlNode item in SkillPropertyList)
        {
            switch(item.Name)
            {
                case "name":
                    skill.name = item.InnerText;
                    break;
                case "damage":
                    skill.damage = item.InnerText;
                    break;
            }
        }
        //将每个skill信息存放到SkillList 列表中
        SkillList.Add(skill);
    }

    foreach (SkillBean item in SkillList)
    {
        Console.WriteLine(item.id+" "+item.name+" "+item.damage);
    }
}
```
TonyChenn

2018.9.7
