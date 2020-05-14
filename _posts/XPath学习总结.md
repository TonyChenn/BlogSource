---
title: xpath学习总结
date: 2018-11-08 19:17:54
tags: 
    - xpath
    - xml
top:
password:
description: 简单粗暴的讲就是--在xml中用来查找信息的语言
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/11.08/icon.jpg
---

## 什么是Xpath?
XPath 是一门在 XML 文档中查找信息的语，Xpath通过使用路径表达式，实现选取XML文档中的一个或多个节点集合。

## 选取节点

|xpath|举例|描述|
|---|---|---|
|div|xpath('div')|选取所有div节点|
|/|xpath('/')|从根节点进行选取|
|//div|xpath('//div')|选取所有div节点，不考虑div的位置|
|.|xpath('./')|选取当前节点|
|..|xpath('../')|选取当前节点的父节点|


## 谓语
|xpath|举例|描述|
|---|---|---|
|/div/a[1]|xpath('/div/a[1]')|选取div节点下的第一个a节点|
|/div/a[last]|xpath('/div/a[last]')|选取div节点下的最后一个a节点|
|/div/a[position<3]|xpath('/div/a[position<3]')|选取div节点下的前2个节点|
|@|xpath('//div[@id]')|选取所有含有id属性的div节点|
||xpath("//div[@id='main']")|选取所有div节点中含有id='main'的节点|
|/div[id<7]/a|xpath('/div[id<7]/a')|选取id值小于7的div节点下的所有a节点|


## 选取未知节点
|xpath|描述|
|---|---|
|'/div/*' |选取div节点下的所有子节点|
|'/div[@*]'|选取所有含有属性的div节点|

## 多路径选择

> 使用 '|' 符号，选取若干路径
```xpath
xpath(xpath1 + '|' + xpath2)
```

## 常用函数
|表达式|用法|解释|
|---|---|---|
|start-with|xpath("//div[start-with(@id,'ma')]")|选取id值以ma开头的div节点|
|contains|xpath("//div[contains(@id,'aa')]")|选取id值中包含aa的div节点|
|and|xpath("div[contains(@id,'xx') and contains(@class,'yy')]")|
|text()|xpath("//div[contains(text(),'zz')]") |选取节点文本包含ma的div节点|

## 案例：
[CSharp-使用HtmlAgilityPack实现网站爬虫总结](https://tonychenn.cn/2018/09/01/CSharp-使用HtmlAgilityPack实现网站爬虫总结/)

## 参考：
> https://blog.csdn.net/gongbing798930123/article/details/78955597
<br>https://www.runoob.com/xpath/xpath-examples.html

<div align='right'>但愿日子清净，款款落落拱手相让的都是柔情。</div>
<div align='right'>TonyChenn<br>2018/11/8</div>