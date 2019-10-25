---
title: MySQL-数据库
date: 2018-04-23 12:21:46
tags: MySQL
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2017/11.06/Mysql.png
---

# 数据库操作
## 创建数据库：
```sql
create database database_name;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/create.jpg)


## 显示所有数据库：
```sql
show databases;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/showall.jpg)


## 选择使用数据库：
```sql
use databsae_name;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/select.jpg)


## 删除数据库：
```sql
drop database database_name;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/delete.jpg)


# 表操作
## 显示所有表：
```sql
show tables;
```
## 创建表：

//创建country表

//包含id ，name字段

//id 自增，并且作为主键
```sql
create table country(id int auto_increment ,name varchar(30), primary key(id) );  
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/createtable.jpg)


## 查看表结构：
```sql
desc table_name;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/tablestruct.jpg)


# 数据操作

## 添加信息：

//添加一个国家：中国；
```sql
insert into table_name(name) values ('china');
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/add.jpg)


## 修改表内数据
```sql
update table_name set item1=value1 where condition;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/edit.jpg)


## 删除表中数据：
```sql
delete from table_name where condition;
```
![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/deletetable.jpg)

## 查询表内所有数据:
```sql
select * from table_name;
```
## 查询指定条件数据：
```sql
select * from country where condition;
```
## 查询前n条数据：
```sql
select * from table_name limit n;  
```

## 跳过前m条，查询后面的n条数据：
```sql
select * from table_name limit m,n;
```
## 查找不包含condition的数据：
```sql
select * from table_name where id <> 1;  
```

## 查找既满足条件A,又满足条件B的数据：
```sql
select * from table_name where id between A and B;  
```

## 查找满足条件A或者满足条件B的0数据：
```sql
select * from table_name where condition1 or condition 2;  
```

## 查找数据在条件（a->b之间）的数据:
```sql
select * from table_name where id in(a,b);  
```

## 模糊查询:
```sql
select * from table_name where name like 'A%';  
select * from table_name where name like 'A_';  
```

## 表内数据倒序排序
```sql
select * from country order by id desc;  
```

![](https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.23/desc.jpg)

**5.1.表内数据正序排序**  
```sql
select * from country order by id ;  
```
<div align="right">
2018.4.23

Tony-Chen

一个人熬过了所有苦难，也就不在期待一定要和谁在一起了。努力过，但结局我不想说。
</div>