---
title: android-简易发短信软件
date: 2017-07-09 18:31:30
tags: Android
img: 
---
 **MainActivity.java**

 
```java
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.telephony.SmsManager;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    private Button send;
    private EditText ed1,ed2;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        send= (Button) findViewById(R.id.send);
        send.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                ed1= (EditText) findViewById(R.id.receiver);
                ed2= (EditText) findViewById(R.id.message);
                String number=ed1.getText().toString();
                String text=ed2.getText().toString();
                SmsManager sms= SmsManager.getDefault();
                sms.sendTextMessage(number,null,text,null,null);
            }
        });
    }
}
```
 **activity_main.xml**

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center|left"
    android:weightSum="1">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="短信发送器"
        android:textColor="@color/colorAccent"
        android:textSize="24sp"
        android:textStyle="bold" />

    <EditText
        android:id="@+id/receiver"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:hint="接收者："
        android:inputType="phone" />

    <EditText
        android:id="@+id/message"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:hint="请输入短信内容：" />

    <Button
        android:id="@+id/send"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="发送" />

</LinearLayout>

```
 添加发短信权限

 
```xml
<uses-permission android:name="android.permission.SEND_SMS"></uses-permission>
```

 Tony-Chenn 编写   
 转载请说明出处   
 20170709

   
  