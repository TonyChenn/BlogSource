---
title: Android-悬浮按钮
date: 2017-08-06 17:34:13
tags:
    - Android
    - MaterialDesign
description: MD 库中的一个浮动按钮
img: https://i.loli.net/2019/04/28/5cc54d999675a.jpg
---

```xml
<android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        android:onClick="fabClick"
        app:backgroundTint="@android:color/holo_blue_dark"
        app:srcCompat="@mipmap/icon_serch" />
```
 
```java
FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
```
   
  