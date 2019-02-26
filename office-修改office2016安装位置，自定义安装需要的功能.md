---
title: office-修改office2016安装位置，自定义安装需要的功能
date: 2018-06-22 14:42:27
tags: office
---
下载新版本office2016后发现安装过程

 **没有自定义安装**   
 **默认安装路径在C盘**

 而我只需要安装Word,PowerPoint,Excel,其他功能都用不着   
 C盘也快被撑爆了

 **步骤：**

 1.office2016下载地址[https://msdn.itellyou.cn/](https://msdn.itellyou.cn/)   
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt952m2vj30s20dxtb7.jpg) 

下载后解压（我的解压在桌面）

 **2.**下载并运行微软提供的[Office2016部署工具](https://www.microsoft.com/en-us/download/confirmation.aspx?id=49117) 
 会生成2个文件：   
 setup.exe   
 configuration.xml   
 使用文本编辑器打开configuration.xml 找到< Configuration>节点   
 把下面代码替换到你的Configuration节点下

 
```xml
<Configuration>
  <Add SourcePath="C:\Users\tc\Desktop\cn_office_2016_dvd\" OfficeClientEdition="64" >
    <Product ID="ProPlusRetail">
        <Language ID="zh-CN" />
        <ExcludeApp ID="Access" />
        <ExcludeApp ID="Groove" />
        <ExcludeApp ID="InfoPath" />
        <ExcludeApp ID="Lync" />
        <ExcludeApp ID="OneNote" />
        <ExcludeApp ID="Outlook" />
        <ExcludeApp ID="Publisher" />
        <ExcludeApp ID="Skype for Business" /> 
        <ExcludeApp ID="SharePointDesigner" />
    </Product>
  </Add>
</Configuration>
```
 解释下   
 SourcePath： office镜像解压后的目录   
 OfficeClientEdition：64位或32位   
 Language ID： 语言   
 ExcludeApp ID： 不安装的功能（只剩下Word,PowerPoint,Excel）

 保存，关闭。**到此完成只安装Word,PowerPoint,Excel**

 3.实现安装在非系统盘：   
 Win+R 输入regedit,打开编辑注册表   
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt9cz7huj30e907k3yr.jpg) 
 找到 计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion   
 把下面图片中的三个改成你想要安装的目录:   
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt9tldduj30jh06n74s.jpg) 
 **到此完成安装目录设置**

 4.开始安装：   
 Win+R输入cmd

 
```
//打开桌面目录
//就是setup.exe 和 configuration.xml 所在目录
cd desktop

//开始安装
setup.exe /configure configuration.xml

```
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt9tldduj30jh06n74s.jpg)

 等待安装完成   
 。   
 。   
 。   
 <b>
 安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。   
 安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。   
 安装完成后重启，将上面修改注册表的三个目录重新修改成原来的。(重要事情说三遍)
</b>

 到此安装完成

 5.此时点击图标会发现不能运行，你心里肯定在想着，卧槽，博主，你是不是在耍我，不能运行；

 淡定，下面开始解决无法运行问题

 打开：C:\ProgramData\Microsoft\Windows\Start Menu\Programs\ 目录   
 依次右键Word,PowerPoint,Excel，点击属性   
 如图替换成自己的安装目录对应位置   
 
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furtaf5yrij30f30lfwf6.jpg)
 完成！

 2018.6.22

 没有结果的过程都是垃圾，但偶尔我也会用尽力就好来安慰自己。————-TonyChen