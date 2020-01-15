---
title: android-调用系统拨号器进行拨号
date: 2017-07-09 17:22:42
tags: Android
description: 通过安卓程序调用系统拨号器，进而实现拨号操作
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/03.16/icon.jpg
---

 **MainActivity.java**

 
```java
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    private Button call;
    private TextView tv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tv=(TextView)findViewById(R.id.et1);
        call=(Button)findViewById(R.id.call);
        call.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String phone=tv.getText().toString();       //得到电话号码
                Intent intent=new Intent();
                intent.setAction(Intent.ACTION_CALL);
                intent.setData(Uri.parse("tel:"+phone));
                MainActivity.this.startActivity(intent);
            }
        });
    }
}
```
 **activity_main.xml**

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:layout_gravity="center"
    tools:context="com.example.tony_chen.call_phone.MainActivity">

    <TextView
        android:layout_width="386dp"
        android:layout_height="37dp"
        android:text="迷你拨号器："
        android:textColor="@color/colorAccent"
        android:textSize="30sp"
        android:textStyle="italic"
        tools:layout_editor_absoluteX="0dp"
        tools:layout_editor_absoluteY="60dp" />
    <EditText
        android:id="@+id/et1"
        android:layout_width="366dp"
        android:layout_height="42dp"
        android:hint="请输入手机号"
        android:inputType="phone"
        tools:layout_editor_absoluteX="0dp"
        tools:layout_editor_absoluteY="111dp" />
    <Button
        android:id="@+id/call"
        android:layout_width="142dp"
        android:layout_height="50dp"
        android:text="拨出"
        tools:layout_editor_absoluteX="241dp"
        tools:layout_editor_absoluteY="158dp" />
 </LinearLayout>
```
 **AndroidManifest.xml**

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.tony_chen.call_phone">
//添加权限
    <uses-permission android:name="android.permission.CALL_PHONE"/>
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```
![](https://i.loli.net/2019/04/28/5cc54bf59b962.jpg)

 **整理：Tony-Chen   
 转载请说明出处**

   
  