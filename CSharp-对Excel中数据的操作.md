---
title: CSharp-对Excel中数据的操作
date: 2018-09-25 21:00:08
tags: CSharp
password:
top:
description: 嗯，生活要对Excel动刀子了
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.25/excel.jpg
---

对于新版本，和老版本Excel创建的xls(xlsx)版本，需要用不同的连接字符串,自行选择
```csharp
//老
string conStr = "Provider=Microsoft.Jet.OLEDB.4.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\"Excel 8.0;HDR=YES;IMEX=1\"";
//新
string conStr="Provider=Microsoft.ACE.OLEDB.12.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\"Excel 12.0;HDR=YES;IMEX=1\"";

```


```csharp
string fileName = "装备信息.xls";
//连接字符串
string conStr = "Provider=Microsoft.Jet.OLEDB.4.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\"Excel 8.0;HDR=YES;IMEX=1\"";
//创建连接对象
OleDbConnection connection = new OleDbConnection(conStr);
//打开连接
connection.Open();
//SQL 语句
string sql = "select * from [Sheet1$]";

//查询适配器
OleDbDataAdapter adapter = new OleDbDataAdapter(sql, connection);

//创建数据集
DataSet ds = new DataSet();
//填充数据到数据集
adapter.Fill(ds);
//关闭连接
connection.Close();

//到此拿到了从Excel中的数据
//解析数据
//获取数据集中所有的表格
DataTableCollection tableCollection= ds.Tables;

//当前只有一张表
//拿到这张表
DataTable table = tableCollection[0];

//获取表格中的数据
//获取表格中的每一行
DataRowCollection rows= table.Rows;

///输出数据
foreach (DataRow row in rows)
{
    for (int i = 0; i < 7; i++)
    {
        Console.Write("{0}",row[i]);
    }
    Console.WriteLine();
}
```
输出内容：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/09.25/result.jpg)


> 参考：SikiC#高级课程

TonyChenn

2018.9.25