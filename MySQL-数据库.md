---
title: MySQL-数据库
date: 2018-04-23 12:21:46
tags: MySQL
---
![](https://labs.mysql.com/common/logos/mysql-logo.svg?v2)
<!--more-->

# 数据库操作
## 创建数据库：
```sql
create database database_name;
```
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt78jcr2j30ae01p3yi.jpg)


## 显示所有数据库：
```sql
show databases;
```
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt7e28apj306506jwei.jpg)


## 选择使用数据库：
```sql
use databsae_name;
```
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt7im4yyj3051019wea.jpg)


## 删除数据库：
```sql
drop database database_name;
```
![](https://ww1.sinaimg.cn/mw690/006PThdlly1furt7mnopbj30a501aweh.jpg)


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
![](https://ww1.sinaimg.cn/large/006PThdlly1furt3fr69yj307x03g3yi.jpg) 


## 查看表结构：
```sql
desc table_name;
```
![](https://ww1.sinaimg.cn/large/006PThdlly1furt40ncaoj30hs03eq31.jpg)


# 数据操作

## 添加信息：

//添加一个国家：中国；
```sql
insert into table_name(name) values ('china');
```
![](https://ww1.sinaimg.cn/large/006PThdlly1furt5bya6qj30e301iaa3.jpg)


## 修改表内数据
```sql
update table_name set item1=value1 where condition;
```
![](https://ww1.sinaimg.cn/large/006PThdlly1furt5hd4kvj30ei0cyt9b.jpg)


## 删除表中数据：
```sql
delete from table_name where condition;
```
![](https://ww1.sinaimg.cn/large/006PThdlly1furt5mo4zqj30dt06dweq.jpg)

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

![](https://ww1.sinaimg.cn/large/006PThdlly1furt5qz1xbj30cu0613yn.jpg)

**5.1.表内数据正序排序**  
```sql
select * from country order by id ;  
```
<div align="right">
2018.4.23

Tony-Chen

一个人熬过了所有苦难，也就不在期待一定要和谁在一起了。努力过，但结局我不想说。
</div>