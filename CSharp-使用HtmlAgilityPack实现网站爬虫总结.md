---
title: CSharp-使用HtmlAgilityPack实现网站爬虫总结
date: 2018-09-01 19:22:02
tags: 
 - CSharp 
 - HtmlAgilityPack
---
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fw9tu9essyj31h10o4qmz.jpg)
<!--more-->
前段时间在UWP中需要解析网页上Html文档，找到了[HtmlAgilityPack](https://html-agility-pack.net/)这个工具，网上也给与很高的评价。学习过后总结一下HtmlAgilityPack的使用；

## 工具简介
HtmlAgilityPack是[codeplex里的一款开源库](https://htmlagilitypack.codeplex.com)，是一个灵活的html解析器，支持通过简单XPATH 或 XSLT来读和写DOM，最新版本支持LINQ。

## Xpath
> 移步：[xpath学习总结](https://tonychenn.cn/2018/11/08/XPath学习总结/)

## Parser

```csharp
//from file
var htmlDoc=new HtmlDocument();
htmlDoc.Load(filePath);

//from String
var htmlDoc=new HtmlDocument();
htmlDoc.LoadHtml(html);

//from web
var client=new HtmlWeb();
var htmlDoc=web.Load(url);
// htmlDoc=await web.LoadFromWebAsync(url);

//from WebBrowser

```

## Select Node(Nodes)
```csharp
//select nodes
var Node=htmlDoc.DocumentNode.SelectNodes(XPath);

//select single node
var Node =htmlDoc.DocumentNode.SelectSingleNode(XPath);
```

## Manipulation(操作)

### Properties(属性)
Name | Description
---|---
InnerHtml | 获取或设置对象的开始和结束标记之间的HTML
InnerText | 获取对象的开始和结束标记之间的文本
OuterHtml | 以HTML格式获取对象及其内容
ParentNode | 获取此节点的父节点

### Methords

Name | Description
---|---
AppendChild() | 添加到当前节点的最后一个子节点的后面
PrependChild | 添加到当前节点的第一个子节点的前面
Clone() | 克隆当前节点
CopyFrom() | 克隆当前节点
CreateNode() | 创建节点
InsertAfter() | 在指定节点后插入节点
InsertBefore | 在指定节点前插入节点
Remove | 只移除当前节点（子节点保留）
RemoveAll | 移除当前节点及子节点
RemoveChild(HtmlNode) | 删除指定节点
ReplaceChild(newNode,oldNode) | 替换节点

## Traversing（遍历）

Name | Description
---|---
ChildNodes | 获取节点的所有子节点
FirstChild | 获取节点的第一个子节点
LastChild | 获取节点的最后一个子节点
NextSibling | 获取紧跟此元素后的HTML节点
ParentNode | 获取此节点的父节点


## 读取节点的内容:
```csharp
static void Main(string[] args)
{
    var html =
      @"<td class=texte width='50%'>
	        <div align=right>Name:<B></B></div>
        </td>
        <td width = '50%''
	        <input class=box value = John>
	        <input class=box value = Tony>
	        <input class=box value = Jams>
        </td>
        <tr Align = center >";
    
    var htmlDoc = new HtmlDocument();
    htmlDoc.LoadHtml(html);

    //获取第一个td标签中div子标签中的值
    var str = htmlDoc.DocumentNode.SelectSingleNode("//td/div").InnerText;
    //获取第二个td标签中input子标签的value属性的值
    //var str = htmlDoc.DocumentNode.SelectSingleNode("//td/input").Attributes["value"].Value;
    if(str!=null)
        Console.WriteLine(str);
}
```

## 修改节点的值

```csharp
//修改节点的值
//before
var node= htmlDoc.DocumentNode.SelectSingleNode("//td/input");
Console.WriteLine(node.Attributes["value"].Value);
//after
node.SetAttributeValue("value", "new name");
Console.WriteLine(node.Attributes["value"].Value);
```

## 参考
>[HtmlAgilityPack 官网](https://html-agility-pack.net/)<br>
[C＃网站爬取心得](https://replay923.github.io/2018/08/01/HtmlAgilityPack/)

<div align='right'>TonyChenn<br>2018/9/1</div>
