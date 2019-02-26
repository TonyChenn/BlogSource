---
title: android-menu的使用
date: 2017-08-17 10:36:29
tags: Android
---
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fw97naao1lj30m80cidi2.jpg)
<!-- more-->

 首先创建一个空白Activity   
 在工程的res/目录下创建menu 文件夹，文件夹下和创建一个Menu resource file   
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fvfzxwyd8ej305903eglg.jpg)

添加两个条目，并添加ID：
 
```xml
<item
        android:id="@+id/add_item"
        android:title="添加"/>
    <item
        android:id="@+id/move_item"
        android:title="删除"/>
```
 然后在Activity 中调用onCreateOptionsMenu方法，把刚才写的menu添加进来，调用onOptionsItemSelected 方法，来实现菜单条目的点击事件

 
```java
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main,menu);
        return true;
    }
```
 
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
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fvfzyp6fk0j309n0ggq31.jpg)
   
  