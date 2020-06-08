---
title: Android-四种基本布局(3)
date: 2020/6/5 16:08
update: 
tags: Android
img: 
description:
top: 
---
# 线性布局(LinearLayout)
线性布局将他包含的所有控件在先行方向上依次排列。通过**orientation**控制排列方式为水平(horizontal)或者竖直(vertical),不指定的情况下默认为水平。

## 重要属性
### android:orientation
- horizontal 水平布局
- vertical 垂直布局

### android:layout_gravity 与android:gravity的区别
|属性|区别|
|---|---|
|layout_gravity|用于指定控件在布局中的对齐|
|gravity|用于指定文字在控件中的对齐方式|

### android:layout_weight
通过该属性可以实现以百分比的方式实现控制子控件的大小，使用**layout_weight**会禁用layout_width或layout_height属性。下面再水平布局下，排列一个EditText,一个Button,其中EditText占据4/5宽度，Button占据1/5宽度。
```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <EditText
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="4"/>
    <Button
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="123"
        android:layout_weight="1"/>
</LinearLayout>
```
![layout_weight](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0605/layout_weight.jpg)

通过只指定EditText的layout_weight，而Button的宽度为指定宽度或者wrap_content时,EditText会填充满屏幕剩余部分，从实现更好的显示效果。

## 注意
- 在LinearLayout的orientation为**horizontal**时，内部控件的宽度不能指定为**match_parent**,因为一个控件就**占满**整个屏幕，其他控件无法排列；当然orientation为**vertical**时，内部控件的高度不能指定为**match_parent**。
- 在LinearLayout的orientation为**horizontal**时，内部控件的垂直方向上的对齐才生效，因为此时水平方向上宽度不能确定；当然orientation为**vertical**时，内部控件的水平方向上的对齐才生效。

# 相对布局(RelativeLayout)
RelativiteLayout可以通过相对定位的方式实现让控件出现在布局的任何位置
### 相对父布局定位
主要参数如下：
|属性|描述|
|---|---|
|layout_centerInParent|在父容器居中|
|layout_alignParentTop|在父容器居顶|
|layout_alignParentBottom|在父容器居底|
|layout_alignParentLeft|在父容器居左|
|layout_alignParentRight|在父容器居右|

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="中心"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignParentLeft="true"
        android:text="左上"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignParentRight="true"
        android:text="右上"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentLeft="true"
        android:text="左下"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:text="右下"/>
</RelativeLayout>
```
效果图如下：
![align_parent](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0605/align_parent.jpg)

### 相对控件定位
主要参数如下：
|属性|描述|
|---|---|
|layout_above|在控件上方|
|layout_below|在控件下方|
|layout_toLeftOf|在控件左侧|
|layout_toRightOf|在控件右侧|

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:id="@+id/center"
        android:text="中心"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/center"
        android:layout_toLeftOf="@id/center"
        android:text="左上"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/center"
        android:layout_toRightOf="@id/center"
        android:text="右上"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/center"
        android:layout_toLeftOf="@id/center"
        android:text="左下"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/center"
        android:layout_toRightOf="@id/center"
        android:text="右下"/>
</RelativeLayout>
```
效果图如下：

![align_copoment](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0605/align_copoment.jpg)

### 相对控件对齐
主要参数如下：
|属性|描述|
|---|---|
|layout_alignLeft|与控件左对齐|
|layout_alignRight|与控件右对齐|
|layout_alignTop|与控件上对齐|
|layout_alignBottom|与控件下对齐|

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:layout_width="300dp"
        android:layout_height="200dp"
        android:layout_centerInParent="true"
        android:id="@+id/center"
        android:text="中心"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@id/center"
        android:text="左对齐"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignRight="@id/center"
        android:text="右对齐"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignTop="@id/center"
        android:text="上对齐"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@id/center"
        android:text="下对齐"/>
</RelativeLayout>
```

![align_copoment_2](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0605/align_copoment_2.jpg)

# 帧布局(FrameLayout)
然并卵...
# 百分比布局
由于只有LinearLayout支持以比例的形式控制控件的大小，RelativeLayout和FrameLayout很难实现百分比控制控件的大小，所以Android引入**百分比布局**的形式解决这个问题。</br>
百分比布局包括1.PercentFrameLayout，2.PercentRelativeLayout. </br>
在百分比布局中不再设置**layout_width**，**layout_height**，而是**layout_widthPercent**与**layout_heightPercent**,为了让所有的Android版本都能用上，所以Android团队将百分比布局定义在了support库中。在使用时在build.gradle中添加即可。
```java

```