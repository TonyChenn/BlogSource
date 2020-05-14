---
title: Android-sweetDialog看着都是甜的
date: 2017-09-12 14:50:35
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/03.16/icon.jpg
---

 ==声明：本篇文章为转载==

 **Android第三方开源对话消息提示框：SweetAlertDialog（sweet-alert-dialog）**

 Android第三方开源对话消息提示框：SweetAlertDialog（sweet-alert-dialog）是一个套制作精美、动画效果出色生动的Android对话、消息提示框，如图所示（部分，还有更多效果，不在此一一展示）：

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/09.12/dialog.gif)

 

![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/09.12/err.gif)

 

 ![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/09.12/loading.gif)

 SweetAlertDialog（sweet-alert-dialog）在github上的项目主页是：[https://github.com/pedant/sweet-alert-dialog](https://github.com/pedant/sweet-alert-dialog)

 

 外层：Gradle

 
``` java
repositories {
    mavenCentral()
}
```
 

app下的gradle:
```
dependencies { compile ‘cn.pedant.sweetalert:library:1.3’}
```
 导入后只要设置非默认图标的会有一坑，参考：

 [https://blog.csdn.net/wf632856695/article/details/51736291](https://blog.csdn.net/wf632856695/article/details/51736291)

 显示Material进度样式

 
```
SweetAlertDialog pDialog = new SweetAlertDialog(this, SweetAlertDialog.PROGRESS_TYPE);
pDialog.getProgressHelper().setBarColor(Color.parseColor("#A5DC86"));
pDialog.setTitleText("Loading");
pDialog.setCancelable(false);
pDialog.show();

```
 [![](https://ws1.sinaimg.cn/large/006PThdlly1fvftwxgkaqg30ak07sgpc.gif)](https://github.com/pedant/sweet-alert-dialog/raw/master/play_progress.gif)

 只显示标题：

 
```
new SweetAlertDialog(this)
    .setTitleText("Here's a message!")
    .show();

```
 显示标题和内容：

 
```
new SweetAlertDialog(this)
    .setTitleText("Here's a message!")
    .setContentText("It's pretty, isn't it?")
    .show();

```
 显示异常样式：

 
```
new SweetAlertDialog(this, SweetAlertDialog.ERROR_TYPE)
    .setTitleText("Oops...")
    .setContentText("Something went wrong!")
    .show();

```
 显示警告样式：

 
```
new SweetAlertDialog(this, SweetAlertDialog.WARNING_TYPE)
    .setTitleText("Are you sure?")
    .setContentText("Won't be able to recover this file!")
    .setConfirmText("Yes,delete it!")
    .show();

```
 显示成功完成样式：

 
```
new SweetAlertDialog(this, SweetAlertDialog.SUCCESS_TYPE)
    .setTitleText("Good job!")
    .setContentText("You clicked the button!")
    .show();

```
 自定义头部图像：

 
```
new SweetAlertDialog(this, SweetAlertDialog.CUSTOM_IMAGE_TYPE)
    .setTitleText("Sweet!")
    .setContentText("Here's a custom image.")
    .setCustomImage(R.drawable.custom_img)
    .show();

```
 确认事件绑定：

 
```
new SweetAlertDialog(this, SweetAlertDialog.WARNING_TYPE)
    .setTitleText("Are you sure?")
    .setContentText("Won't be able to recover this file!")
    .setConfirmText("Yes,delete it!")
    .setConfirmClickListener(new SweetAlertDialog.OnSweetClickListener() {
        @Override
        public void onClick(SweetAlertDialog sDialog) {
            sDialog.dismissWithAnimation();
        }
    })
    .show();

```
 显示取消按钮及事件绑定：

 
```
new SweetAlertDialog(this, SweetAlertDialog.WARNING_TYPE)
    .setTitleText("Are you sure?")
    .setContentText("Won't be able to recover this file!")
    .setCancelText("No,cancel plx!")
    .setConfirmText("Yes,delete it!")
    .showCancelButton(true)
    .setCancelClickListener(new SweetAlertDialog.OnSweetClickListener() {
        @Override
        public void onClick(SweetAlertDialog sDialog) {
            sDialog.cancel();
        }
    })
    .show();

```
 确认后切换对话框样式：

 
```
new SweetAlertDialog(this, SweetAlertDialog.WARNING_TYPE)
    .setTitleText("Are you sure?")
    .setContentText("Won't be able to recover this file!")
    .setConfirmText("Yes,delete it!")
    .setConfirmClickListener(new SweetAlertDialog.OnSweetClickListener() {
        @Override
        public void onClick(SweetAlertDialog sDialog) {
            sDialog
                .setTitleText("Deleted!")
                .setContentText("Your imaginary file has been deleted!")
                .setConfirmText("OK")
                .setConfirmClickListener(null)
                .changeAlertType(SweetAlertDialog.SUCCESS_TYPE);
        }
    })
    .show();
```
   
 环形进度条变换颜色：

 

 
```
final SweetAlertDialog pDialog = 
    new SweetAlertDialog(this, SweetAlertDialog.PROGRESS_TYPE)                 
    .setTitleText("Loading");         
pDialog.show();         
pDialog.setCancelable(false);         
new CountDownTimer(800 * 7, 800) {             
    public void onTick(long millisUntilFinished) {                 
        // you can change the progress bar color by ProgressHelper every 800 millis
        i++;                 
        switch (i) {                     
            case 0:                         
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.blue_btn_bg_color));                         
            break;
                                 
            case 1:                         
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.material_deep_teal_50));
             break;                     
             
             case 2:                         
             pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.success_stroke_color));
            break; 
                                
            case 3:
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.material_deep_teal_20));
            break;

            case 4:
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.material_blue_grey_80));
            break;
            
            case 5:
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.warning_stroke_color));
            break;

            case 6:
            pDialog.getProgressHelper()
                .setBarColor(getResources()
                .getColor(R.color.success_stroke_color));
            break;
        }
    }
    
    public void onFinish() {
        i = -1;
        pDialog.setTitleText("Success!")
        .setConfirmText("OK")
        .changeAlertType(SweetAlertDialog.SUCCESS_TYPE);
    }
}.start(); 
```