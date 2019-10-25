---
title: Android-RelativeLayout
date: 2017-10-07 22:07:04
tags: Android
img: https://i.loli.net/2019/04/28/5cc54fe8585bf.jpg
---

 **相对布局常用属性：**

 **子类控件相对子类控件：值是另外一个控件的id**
<!-- more -->
```xml
android:layout_above----------位于给定DI控件之上
android:layout_below ----------位于给定DI控件之下

android:layout_toLeftOf -------位于给定控件左边
android:layout_toRightOf ------位于给定控件右边

android:layout_alignLeft -------左边与给定ID控件的左边对齐
android:layout_alignRight ------右边与给定ID控件的右边对齐
android:layout_alignTop -------上边与给定ID控件的上边对齐
android:layout_alignBottom ----底边与给定ID控件的底边对齐

android:layout_alignBaseline----对齐到控件基准线
```
 **相对父容器，值是true或false**

 
```xml
android:layout_alignParentLeft ------相对于父靠左
android:layout_alignParentTop-------相对于父靠上
android:layout_alignParentRight------相对于父靠右
android:layout_alignParentBottom ---相对于父靠下

android:layout_centerInParent="true" -------相对于父即垂直又水平居中
android:layout_centerHorizontal="true" -----相对于父即水平居中
android:layout_centerVertical="true" --------相对于父即处置居中
```
 **相对于父容器位置：**

 
```
android:layout_margin="10dp"
android:layout_marginLeft
android:layout_marginRight
android:layout_marginTop
android:layout_marginBottom
```
 **版本4.2以上相对布局新属性**

 
```
android:layout_alignStart---------------------将控件对齐给定ID控件的头部
android:layout_alignEnd----------------------将控件对齐给定ID控件的尾部
android:layout_alignParentStart--------------将控件对齐到父控件的头部
android:layout_alignParentEnd---------------将控件对齐到父控件的尾部
```
 安卓API更新很快，不能保证代码当你看到的时候依然可以使用~
 
 Good Night~