---
title: Android-Animations（一）
date: 2017-08-25 00:18:10
tags: Android
description: Tweened Animations提供了旋转，移动，伸展和淡入淡出动画效果.
---
{# cq #}
 **1.Alpha 淡入淡出效果**   
 **2.Scale 缩放效果**   
 **3.Rotate 旋转效果**   
 **4.Translate 移动效果**
 {# endcq #}
<!-- more -->
 **使用步骤：**   
 1. 创建AnimationSet对象；_   
 2. 根据需求建立相应Animation 对象；_   
 3. 根据动画需求，为Animation对象设置相应数据；_   
 4. 将Animation对象添加到AnimationSet对象中；_   
 5. 使用控件对象开始执行AnimationSet；_

 **通用属性：**

 
```java
1.setDuration(long durationMills)       
//------------------------设置动画持续时间

2.setFillAfter(boolean fillAfter)
    //............true-------动画执行后，停留在执行结束位置
    //............false------动画执行后，返回到执行前的位置

3.setFillBefore(boolean fillBefore)
//............true-------动画执行前，返回到执行前的位置
//............false------动画执行前，停留在执行结束位置
4.setStartOffSet(long startOffSet)
//--------------设置动画执行前等待时间

5.SetRepeatCount(int repeatCount)
//-------------------设置动画重复次数
```
 **1.淡入淡出动画**

 
```java
//创建AnimationSet对象
AnimationSet animationSet=new AnimationSet(true);
//创建AlphaAnimation对象
//1 0 fromAlpha 1-> toAlpha 0
AlphaAnimation alphaAnimation=new AlphaAnimation(1,0);
//动画执行时间
alphaAnimation.setDuration(1000);
//
animationSet.addAnimation(alphaAnimation);
img.startAnimation(animationSet);
```
 **2.缩放效果**

 
```java
AnimationSet animationSet=new AnimationSet(true);
/*
* 8个参数
* 1.X轴从多大
* 2.X轴缩小到多大
* 3.Y轴多大
* 4.Y轴缩小到多大
* 5.6.7.8.......
*
* */
ScaleAnimation scaleAnimation=new ScaleAnimation(1,0.1f,
               1,0.1f,
               Animation.RELATIVE_TO_SELF,0.5f,
               Animation.RELATIVE_TO_SELF,0.5f);
animationSet.addAnimation(scaleAnimation);
animationSet.setDuration(1000);
img.startAnimation(animationSet);
```
 **3.旋转效果**

 
```java
AnimationSet animationSet=new AnimationSet(true);
/*
* 6个参数
*
* 1.从哪个角度开始
* 2.到那个角度结束
* 3.X轴的坐标值      Animation.RELATIVE_TO_SELF         旋转的圆心在自身
*                  Animation.RELATIVE_TO_PARENT       旋转圆心在父控件上
* 4.（1f）自身或父控件X坐标1倍
* 5.Y轴的坐标值
* 6.
* */
RotateAnimation rotateAnimation=new RotateAnimation(0,360,
        Animation.RELATIVE_TO_PARENT,1f,
        Animation.RELATIVE_TO_PARENT,0f);
rotateAnimation.setDuration(1000);
animationSet.addAnimation(rotateAnimation);
img.startAnimation(animationSet);
```
 4.移动效果

 
```java
AnimationSet animationSet=new AnimationSet(true);
TranslateAnimation translateAnimation=new TranslateAnimation(
        Animation.RELATIVE_TO_SELF,0f,
        Animation.RELATIVE_TO_SELF,0.5f,
        Animation.RELATIVE_TO_SELF,0f,
        Animation.RELATIVE_TO_SELF,1.0f);
translateAnimation.setDuration(1000);
animationSet.addAnimation(translateAnimation);
img.startAnimation(animationSet);
```
   
  