---
title: CSharp连接MySQL
date: 2017-11-06 21:37:11
tags: 
     - CSharp
     - MySQL 
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.06/Mysql.png
---

# 第一步：
 下载MySql，安装好MySql Server，MySql For VisualStudio两项；   
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.06/install.jpg) 
 这是必不可少的｛/滑稽｝不多说

# 第二步  
 打开VisualStudio，创建一个窗体应用程序；   
 在服务器资源管理器上创建连接：   
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.06/Conntct.jpg)

# 第三步：   
 下载文件：MySql.Data.dll(资源尚在审核中……..)
 直接在NuGet上搜索MySQL添加即可  
 （没有此文件，下面无法找不到MySql.Data.MySqlClient 这个包）

# 第四步：   
 添加命名空间：using MySql.Data.MySqlClient;

# 连接文件：
```csharp
string constr = "server=localhost;User Id=root;password=*******;Database=DatabaseName";
MySqlConnection mycon = new MySqlConnection(constr);
mycon.Open();
MySqlCommand mycmd = new MySqlCommand("insert into lost(OBName,OBType,StuName) values('校园卡','卡品','Jack')", mycon);
if (mycmd.ExecuteNonQuery() > 0)
{
   MessageBox.Show("数据插入成功！");
}
mycon.Close();
```
 ok，到此结束。

 2017.11.6   
 Tony-Chen

 **留人间多少爱，迎浮世千重变。**