---
title: CSharp-CSDN博客导出工具
date: 2018-08-30 14:07:49
tags: 
    - CSharp
    - 爬虫
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/icon.png
---
# 博客导出工具

# 更新介绍（2019.9.26）
  1. 新的UI交互界面 
  2. 解决由于文件名导致的无法写文件的问题 
  3. 操作更加简便，可以选择下载，添加鼠标右击菜单 
  4. 以后会集成更多的博客平台  

# 2019.10.15
解决CSDN反爬虫问题

# 免责声明

 工具免费，请不要用于侵权等各种行为，对于违法使用此工具所造成后果，本工具作者不负任何责任。

 工具免费，请不要用于侵权等各种行为，对于违法使用此工具所造成后果，本工具作者不负任何责任。

 工具免费，请不要用于侵权等各种行为，对于违法使用此工具所造成后果，本工具作者不负任何责任。

 重要事情说三遍

 
# 介绍：

 前段时间使用[Github](https://github.com/)+[Hexo](https://hexo.io)搭建了自己的博客,（顺便加个广告：我的博客：[TonyChenn.github.io](TonyChenn.github.io)），之前一直使用的CSDN写博客，想把CSDN上的博文导出为MarkDown格式的文档，在上传到自己的博客上。在网上找到一些用Python写的工具，经过实验都已不能用。所以在研究了下CSDN的网页结构后， 决定自己动手丰衣足食。  
 ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/001.png)

 
# 导出效果：

 
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/002.jpg)  
 文档内含有**博文标题**，**创建时间**，**Tags**,如下：  
 ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/003.jpg)

 
### []()注意

  
  * URL地址输入请按照上面图片中的格式 
  * 运行环境 Win10,如不能使用请[GitHub](https://TonyChenn.cn)联系我 
  * .Net 版本 4.6 
  * CSDN图床中图片部署在hexo上，在电脑中可显示出来，在手机上显示不出来，建议替换图片  
--------
 
### []()使用方法

1. 选择平台
2. 粘贴对应网址
3. 点击解析
4. CSDN下载需要配置UserName和UserToken;(待优化为登陆)

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/004.png)
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/08.30/005.png)

--------
 如链接不能使用，或者工具失效，请与我联系。也欢迎提出各种宝贵意见。