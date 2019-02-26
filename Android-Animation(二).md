---
title: Android-Animation(二)
date: 2017-08-25 09:26:46
tags: Android
---
 在第一篇中，Animation(一)中通过在程序中实现动画效果。这一篇则通过xml文件的方式实现Animation动画。

 各有优缺点，根据实际情况，选择不同方法。

 **1.在工程的res/目录下添加anim文件夹**   
 **2.在res/anim/下创建一个xml文件。**

 
```xml
<set xmlns:android ="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/accelerate_interpolator">
</set>
```
 **3.在标签中添加rotate，alpha，scale，translate标签；**

 **3.1.Alpha.xml**

```xml
<alpha android:fromAlpha="1.0"
    android:toAlpha="0.0"
    android:startOffset="500"
    android:duration="500"/>
```
 **3.2.Rotate.xml**

 
```xml
<!--
    android:pivotX三种设置方法

    1.android:pivotX=“50”       使用绝对位置定位
    2.android:pivotX=“50%”      相对于控件本身定位
    3.android:pivotX=“50%p”     相对于父控件定位
    -->
    <rotate
        android:fromDegrees="0"
        android:toDegrees="180"
        android:pivotX="50%"
        android:pivotY="50%"
        android:duration="1000" />
```
 **3.3.Scale.xml**

 
```xml
    <scale
        android:fromXScale="1.0"
        android:toXScale="0.0"
        android:fromYScale="1.0"
        android:toYScale="0.0"
        android:pivotX="50%"
        android:pivotY="50%"
        android:duration="1000"/>
```xml
 **3.4Translate.xml**

 
```xml
    <translate
        android:fromXDelta="50%"
        android:toXDelta="100%"
        android:fromYDelta="50%"
        android:toYDelta="100%"
        android:duration="1000"/>
```
 **4.在代码中使用AnimationUtils 中装载xml文件，并生成Animation对象；**
 
```java
Animation animation=AnimationUtils.loadAnimation(MainActivity.this,R.anim.alph);
img.startAnimation(animation);
```
   
  