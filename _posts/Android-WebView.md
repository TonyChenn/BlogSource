---
title: Android-WebView
date: 2017-10-11 13:06:27
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/03.16/icon.jpg
---

浏览器主要是使用WebView组件

 先看下简单的效果![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/10.11/webview.jpg)

 只实现了一些简单的浏览器功能
 
```java
WebView webView;
TextView tip;

tip= (TextView) findViewById(R.id.tip);
        webView= (WebView) findViewById(R.id.web_view);
        WebSettings webSettings=webView.getSettings();          //设置浏览器属性
        webSettings.setJavaScriptEnabled(true);       //设置支持JavaScript
        //缩放操作
        webSettings.setSupportZoom(true); //支持缩放，默认为true。是下面那个的前提。
        webSettings.setBuiltInZoomControls(true); //设置内置的缩放控件。若为false，则该WebView不可缩放
        webSettings.setDisplayZoomControls(false); //隐藏原生的缩放控件


        webView.setWebViewClient(new WebViewClient());
        //加载网页
        webView.loadUrl("https://www.baidu.com");
        //加载apk内资源
        //webView.loadUrl("file:///android_asset/example.html");

        webView.setWebChromeClient(new WebChromeClient() {
            @Override
            public void onProgressChanged(WebView view, int newProgress) {
                // TODO Auto-generated method stub
                if (newProgress == 100) {
                    //
                    tip.setText("网页加载完成");
                    tip.setVisibility(View.GONE);
                } else {
                    tip.setText("正在加载..."+newProgress+"%");
                }
            }
        });
```
 按钮点击事件

 
```java
    //回退
    public void beforeClick(View view){
        if(webView.canGoBack()){
            webView.goBack();
        }
    }
    //前进
    public void afterClick(View view){
        if(webView.canGoForward()){
            webView.goForward();
        }
    }
    //退出
    public void exitClick(View view){
        RelativeLayout relativeLayout= (RelativeLayout) findViewById(R.id.mLayout);
        //移除webView
        relativeLayout.removeView(webView);
        //清除历史缓存
        webView.clearHistory();
        //销毁webView
        webView.destroy();
        System.exit(0);
    }
```
 布局：

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:id="@+id/mLayout"
    tools:context="demo.dtstudio.com.networkdemo.MainActivity">

    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/web_view"/>
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:color/white"
        android:id="@+id/tip"/>
    <LinearLayout
        android:layout_alignParentBottom="true"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:onClick="beforeClick"
            android:text="后退"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:onClick="afterClick"
            android:text="前进"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:onClick="exitClick"
            android:text="退出"/>
    </LinearLayout>

</RelativeLayout>

```