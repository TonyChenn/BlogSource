---
title: Android-Intent相关总结
date: 2020-05-21 18:21:51
updated: 2020-06-04
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/android/intent.jpg
description:
top:
---
Intent 是Android程序各个组件之间进行交互的重要方式，它不仅可以指明当前组件想要执行的动作，还可以在组件之间传递数据。Intent一般可以用于启动活动，启动服务，以及发送广播场景。Intent大致可以分为两种：**1.显式Intent**和**隐式Intent**。

# 实现界面跳转
```java
//从FirstActivity 跳转至 SecondActivity
startActivity(new Intent(FirstActivity.this,SecondActivity.class));
```
# 实现调用浏览器打开网页
```java
Intent intent=new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("https://www.baidu.com"));
startActivity(intent);
```
# 实现调用拨号器打电话
```java
Intent intent=new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:10086"));
startActivity(intent);
```
# 实现Activity间数据传递
## 传递给下一个Activity数据
下面是实现点击按钮传递链接给自定义的WebView并打开链接核心代码。（记得添加互联网权限）
1. 传递数据
```java
Intent intent=new Intent(FirstActivity.this, WebViewActivity.class);
//通过key,value的形式传递值
intent.putExtra("url","https://tonychenn.cn");
startActivity(intent);
```
2. 接收数据
```java
WebView webView=findViewById(R.id.myWebView);
//首先通过获取传递过来的值
Intent intent=getIntent();
String url= intent.getStringExtra("url");
//打开传递过来的URL
webView.setWebViewClient(new WebViewClient());
webView.loadUrl(url);
```
## 返回数据给上一个Activity
在上面的基础上，打开链接后，点击返回按钮，给FirstActivity返回一条消息并提示出来。
与上面不同的是第一个Activity通过**startActivityForResult**启动新的Activity
```java
Intent intent=new Intent(FirstActivity.this, WebViewActivity.class);
intent.putExtra("url","https://tonychenn.cn");
//第二个参数是请求码，int类型，只需要保证其唯一性即可
startActivityForResult(intent,1);
```
然后添加返回数据的监听
```java
//requestCode 就是上面的请求码
//resultCode  有两种 RESULT_OK 和 RESULT_CANCELED
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    switch (requestCode){
        case 1:
            if(resultCode==RESULT_OK){
                String msg=data.getStringExtra("result");
                Toast.makeText(getApplicationContext(), msg, Toast.LENGTH_SHORT).show();
            }
            break;
    }
}
```
在第二个Activity中添加返回键事件
```java
@Override
public void onBackPressed() {
    Intent intent=new Intent();
    //key value
    intent.putExtra("result","这是一条来自WebView的消息");
    //setResult 是专门向上一个活动返回数据的
    setResult(RESULT_OK,intent);
    //关闭当前Activity
    finish();
}
```