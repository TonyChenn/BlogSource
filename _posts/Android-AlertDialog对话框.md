---
title: Android-AlertDialog对话框
date: 2017-08-26 16:19:26
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019.04.28/icon.jpg
---

 **1.普通的：** 
```java
AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
builder.setIcon(R.mipmap.ic_launcher);
builder.setTitle("提醒：");
builder.setMessage("确定删除？");
builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.setNeutralButton("忽略", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.show();
```
 **2. 列表：**

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019.04.28/001.jpg)

 
```java
AlertDialog.Builder builder=new AlertDialog.Builder(MainActivity.this);
builder.setIcon(R.mipmap.ic_launcher);
builder.setTitle("选择：");
//    指定下拉列表的显示数据
final String[] day= {"星期一", "星期二", "星期三", "星期四", "星期五","星期六","星期日"};
//    设置一个下拉的列表选择项
builder.setItems(day, new DialogInterface.OnClickListener()
{
    @Override
    public void onClick(DialogInterface dialog, int i)
    {
        Toast.makeText(MainActivity.this, "选择的是：" + day[i], Toast.LENGTH_SHORT).show();
    }
});
builder.show();
```
 **3.包含单选框**  
 
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019.04.28/002.jpg)
 
```java
AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
builder.setIcon(R.mipmap.ic_launcher);
builder.setTitle("请选择性别");
final String[] sex = {"男", "女"};
/**
    * 第一个参数：数据集合
    * 第二个参数：默认勾选
    * 第三个参数：绑定监听器
    */
builder.setSingleChoiceItems(sex, 1, new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        Toast.makeText(MainActivity.this, "性别：" + sex[i], Toast.LENGTH_SHORT).show();
    }
});
builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.show();
```
 4.包含CheckBox   
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019.04.28/003.jpg)

 
```java
AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
builder.setIcon(R.mipmap.ic_launcher);
builder.setTitle("水果");
final String[] hobbies = {"苹果", "香蕉", "菠萝", "橘子"};
//Boolean[] select={true,false,false,true};
/**
    * 第一个参数：数据集合
    * 第二个参数:如果是null，则一个都不选择，指定多个被选择，则传递一个Boolean数组进去
    * 第三个参数：绑定监听器
    */
builder.setMultiChoiceItems(hobbies, null, new DialogInterface.OnMultiChoiceClickListener() {
    StringBuffer stringBuffer = new StringBuffer(100);
    @Override
    public void onClick(DialogInterface dialog, int i, boolean isChecked) {
        if(isChecked) {
            stringBuffer.append(hobbies[i] + ", ");
        }
        Toast.makeText(MainActivity.this, "爱好为：" + stringBuffer.toString(),
                        Toast.LENGTH_SHORT)
        .show();
    }
});
builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        //TODO
    }
});
builder.show();
```
   
  