---
title: Android-Animation（三）
date: 2017-08-25 10:29:34
tags: Android
---
 这一篇，写一下Interpolator的使用方法

 **Interpolator 定义了动画的变化速率，在Animations框架当中定义了以下几种Interpolator,在xml的set标签中使用；**

 **xml中实现：**

 
```xml
android:interpolator="@android:anim/accelerate_interpolator"
```
 **Activity中实现：**

 
```java
AnimationSet aninationSet=new AnimationSet(true);
animationSet.setInterpolator(new AccelerateInterpolator());
```
- AccelerateDecelerateInterpolator：在动画开始和结束的时候速率改变比较慢，中间时候加速；

- AccelerateInterpolator：在动画开始时候速率改变比较慢，然后开始加速；

- CycleInterpolator：动画循环播放特定次数，速率改变沿着正弦曲线；

 - DecelerateInterpolator：在动画开始的时候速率改变比较快，然后开始减速；

 - LinearInterpolator：动画以均匀的速率改变

 在需要多个动画叠加时：   
 在xml文件的set标签中添加：   
 android:shareInterpolator=”true” //共享动画效果

   
  