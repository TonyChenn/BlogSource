---
title: android-menu的使用(1)
date: 2017-08-17 10:36:29
updatetime: 2020-05-21 18:06:15
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/03.16/icon.jpg
---

 首先创建一个空白Activity   
 在工程的res/目录下创建menu 文件夹，文件夹下并创建一个Menu resource file文件，在里面添加菜单的条目，并添加ID：
 
```xml
<item
        android:id="@+id/add_item"
        android:title="添加"/>
    <item
        android:id="@+id/move_item"
        android:title="删除"/>
```
 然后在Activity 中重写**onCreateOptionsMenu**方法，把刚才写的menu添加进来，即可实现菜单的显示
```java
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main,menu);
        return true;
    }
```
 菜单显示后就通过重写**onOptionsItemSelected**方法，来实现菜单条目的点击事件
```java
@Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId())
        {
            case R.id.add_item:
                Toast.makeText(this,"add",Toast.LENGTH_SHORT).show();
                break;
            case R.id.move_item:
                Toast.makeText(this,"move",Toast.LENGTH_SHORT).show();
                break;
            default:
                break;
        }
        return true;
    }
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/08.17/002.jpg)
   
  