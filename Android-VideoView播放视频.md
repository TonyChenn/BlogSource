---
title: 'VideoView播放视频'
date: 2018-08-29 14:09:02
tags: Android
---
<center>VideoView播放视频</center>

MianActivity.java
```
import android.widget.*;
import android.view.View.*;
import android.view.*;
import android.net.*;
import java.io.*;
public class MainActivity extends Activity
{
    private Button bn1,bn2,bn3;
    private VideoView vv;
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        bn1=(Button) findViewById(R.id.mainButton1);
        bn2=(Button) findViewById(R.id.mainButton2);
        bn3=(Button) findViewById(R.id.mainButton3);
        vv=(VideoView) findViewById(R.id.mainVideoView);
        vv.setVideoURI(Uri.parse("[url]file:///sdcard/123.mp4"));
//视频的路径
        vv.setMediaController(new MediaController(this));
        bn1.setOnClickListener(new OnClickListener(){
        @Override
        public void onClick(View p1){
            vv.start();
        }
        });
        bn2.setOnClickListener(new OnClickListener(){
        @Override
        public void onClick(View p1)
{
            vv.pause();
        }
        });
        bn3.setOnClickListener(new OnClickListener(){
        @Override
        public void onClick(View p1)
        {
            vv.stopPlayback();
        }
        });
    }
}
```

>main.xml


```

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android" 
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">
<LinearLayout
    android:layout_height="wrap_content"
    android:layout_width="wrap_content"
    android:orientation="vertical">
<VideoView
    android:id="@+id/mainVideoView"
    android:layout_width="320dp"
    android:layout_height="260dp"/>
<LinearLayout
    android:layout_height="wrap_content"
    android:layout_width="wrap_content"
    android:orientation="horizontal">
    <Button
        android:layout_height="wrap_content"
        android:text="开始"
        android:layout_width="wrap_content"
        android:id="@+id/mainButton1"/>
    <Button
        android:layout_height="wrap_content"
        android:text="暂停"
        android:layout_width="wrap_content"
        android:id="@+id/mainButton2"/>
    <Button
        android:layout_height="wrap_content"
        android:text="结束"
        a    ndroid:layout_width="wrap_content"
        android:id="@+id/mainButton3"/>
    </LinearLayout>
</LinearLayout>
</LinearLayout>
```
TonyChen

2018.3.16