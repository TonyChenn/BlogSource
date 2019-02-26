---
title: Unity-背包系统
date: 2017-11-30 17:23:43
tags: Unity
---
   
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furtcgpelqj30t60in4di.jpg)
<!--more-->
 点击药品，装备，金钱显示出不同的面板；

 看下目录结构：   
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furtclhvolj30ap0flwfs.jpg)

 为使点击药品，装备，金钱三个按钮具有选项卡，给点击前Image添加Toggle组件，分配Graphic属性为点击后的图片（Selected）;   
 现在三个按钮都能按下，我们需要的是只将点击的那个设置为Sclected效果，所以为父控件加上ToggleGroup组件，给下面Item_1,Item_2…..下的Group为父控件；

 面板上表格布局排列：   
 添加空物体，重命名为Grid，点击添加Component->Layout->GridLayoutGroup,然后就可在下面添加各种物品了；

 下面实现点击按钮后下面面板切换；   
 不同Item下OnValueChanged属性添加对应Panel事件，如下：

 ![](https://ww1.sinaimg.cn/mw690/006PThdlly1furtcpe12gj30ii0catbr.jpg)
 完成;

 有种无法用语言表述出来的样子；写的有点失败；

 Tony-Chen   
 2017.11.30