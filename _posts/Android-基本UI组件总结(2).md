---
title: Android-基本UI控件总结
date: 2020-06-04 18:05:31
updated: 
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/android/copoment.jpg
description: 软件也需要拼脸蛋
top: 
---
# Button(按钮)
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="按钮控件"
    android:textAllCaps="false"
    android:onClick="OnButtonclick"/>
```
- <kbd>onClick</kbd>添加点击事件,Button的三种点击事件请看：[Android-Button控件的三种onClick方法](https://tonychenn.cn/2017/07/30/Android-Button%E6%8E%A7%E4%BB%B6%E7%9A%84%E4%B8%89%E7%A7%8DonClick%E6%96%B9%E6%B3%95/)
# TextView(文本控件)
```xml
<TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="文本控件"
    android:gravity="center"/>
```
- <kbd>text</kbd>属性用来改变文字显示
- <kbd>gravity</kbd>用来控制文字的对齐方式，参数有“top,buttom,left,right,center,center_horizontal,center_vertical”,可以有多个参数，参数间使用<kbd>|</kbd>分割开。其中center相当于“center_horizontal | center_vertical”。
# EditText(文本输入)
```xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:id="@+id/et1"
    android:hint="请输入账号"
    android:maxLines="4"/>
```
- <kbd>hint</kbd>没有输入时的提示
- <kbd>maxLines</kbd>当输入文字行数超过设置的行数后可以滚动显示。
- <kbd>inputType</kbd>输入的类型
- 在java中通过<kbd>getText()</kbd>获取输入的内容.
# ImageView(图片控件)
```xml
<ImageView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@mipmap/ic_launcher"/>
```
- <kbd>src</kbd>指定图片路径
- 在代码中使用<kbd>setImageResources()</kbd>动态设置图片.
# ProgressBar
```xml
<ProgressBar
    android:id="@+id/myProgressBar"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center"/>
```
代码控制进度条的显示与隐藏的主要代码如下：
```java
//show
if(myProgressBar.getVisibility()==View.GONE){
    myProgressBar.setVisibility(View.VISIBLE);
}
//hide
if(myProgressBar.getVisibility()==View.VISIBLE){
    myProgressBar.setVisibility(View.GONE);
}
```
# AlertDialog(对话框)
详细请看[Android-AlertDialog对话框](https://tonychenn.cn/2017/08/26/Android-AlertDialog%E5%AF%B9%E8%AF%9D%E6%A1%86/)
# ProgressDialog
在界面上显示一个含有进度条的对话框，同时屏蔽掉其他控件的交互能力，通常在操作耗时，需要用户等待的时候使用。与AlertDialog使用类似，核心代码如下：
```java
public void ShowProcessDialog(View view) {
    ProgressDialog dialog=new ProgressDialog(TestActivity.this);
    dialog.setTitle("提示");
    dialog.setMessage("正在假装下载请稍后");
    dialog.setCancelable(true);
    dialog.show();
}
```
其中<kbd>setCancelable</kbd>设置为false后，点击返回键无法关闭对话框。