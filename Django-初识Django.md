---
title: 初识Django(一)
date: 2018-10-30 12:29:15
tags: 
    - python
    - Django
category: Django
top:
password:
description:
---

<!--more-->
# 常用命令汇总

|作用|command|
|---|---|
|运行服务器|python manage.py runserver|
|创建项目|python manage.py startapp app_name|
|同步数据库|python manage.py makemigrations <br>python manage.py migrate|
|||


# 初识Django

## 环境配置

### Python 3.7安装及环境配置

### pip安装（cmd下pip命令查看是否安装）

### Django安装
前往[Django官网](https://www.djangoproject.com/download/)查看最新安装命令：
> pip install Django==2.1.2

安装完成后检查是安装成功
> django-admin --version

## 创建第一个Django项目
打开需要创建Django工程的目录：使用下面命令创建一个HelloWorld工程：
> django-admin startproject HelloWorld

创建完成后目录结构：

- HelloWorld 
    - __init__.py
    - setting.py    //设置，配置
    - urls.py       //URL 声明； 一份由 Django 驱动的网站"目录
    - wsgi.py       //一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目
- manage.py

## 运行第一个Django项目
> python manage.py runserver

打开浏览器输入：http://127.0.0.1:8000/

此时发现不能访问，原因是没有开启允许访问。**解决方法**编辑HelloWorld目录下setting.py ，把其中的ALLOWED_HOSTS=[]改成ALLOWED_HOSTS=['*'];再次访问：

![](https://ws1.sinaimg.cn/large/006PThdlly1fwksgysitcj312p0l4wfm.jpg)


# 第一个DjangoApp

## 创建App
```cmd
python manage.py startapp app_name
```
## 注册App
```python
INSTALLED_APPS = [
   ...
   # 激活 ShowProjects App
    'ShowProjects'
]
```

## 配置访问路径
```python
# 导入include包
from django.urls import path, include

urlpatterns = [
   ...
   # ShowProjects/分配App 访问路径
   # namespace 保证在不同App下访问该路径不会出问题
    path('ShowProjects/', include('ShowProjects.urls', namespace='ShowProjects'))
]
```
## 创建分发路由
因为在每个App中又有多个页面，所以需要在‘ShowProjects’下也有一个路由分发

在 `/ShowProjects/` 路径下创建文件：`urls.py `

```python

```
Django基于MTV框架：
- Model  数据模型
- Templates 视图模板
- V



<div align='right'>TonyChenn<br>2018.10.30</div>