---
title: 日期选择器
date: 2018-06-29 17:23:10
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.29/datatimeselect.jpg
---

效果图：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/06.29/datatimeselect.jpg)

 代码实现：

 
```
new DatePickerDialog(MainActivity.this, new DatePickerDialog.OnDateSetListener() {
            @Override
            public void onDateSet(DatePicker datePicker, int i, int i1, int i2) {
                //执行点击确定后的事件
                Toast.makeText(MainActivity.this, i+"/"+(i1+1)+"/"+i2, Toast.LENGTH_SHORT).show();
            }
            //默认时间：2018.6.29
},2018,6,29).show();
```
 浮世万千，吾爱有三，一为日，二为月，三为卿，日为朝，月为暮，卿为朝朝暮暮。   
 TonyChen   
 2018.6.29