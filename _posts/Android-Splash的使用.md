---
title: Android-Splash的使用
date: 2017-07-10 22:11:34
tags: Android
description: 我们都使用过手机QQ，当我们第一次启动时会显示企鹅画面，几秒后进入登录界面，那么今天写的就是实现Splash闪屏效果。
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/07.10/splash.jpg
---
 由于简单就直接贴上核心代码：   
 **Splash.java**

```java
import android.content.Intent;
import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class Splash extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);

        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                startActivity(new Intent(Splash.this,MainActivity.class));
                Splash.this.finish();
            }
        },1500);
    }
}
```
 在Manifest.xml文件中将Splash更改为第一启动界面即可

 
```xml
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        //在这修改
        <activity android:name=".Splash">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        //同时将MainActivity在此处注册
        <activity android:name=".MainActivity"></activity>
    </application>
```
   
  