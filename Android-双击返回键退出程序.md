---
title: 'Android-双击返回键退出程序'
date: 2018-03-16 20:16:58
tags: Android
---
## 前言
在Android中为防止不小心点击到返回键就退出软件，所以通常软件都会使用点击返回键时显示一个Toast消息“再点击一次退出”，当在指定时间内再次点击按钮时退出程序

```java
public class MainActivity extends Activity { 
    @Override public void onCreate(Bundle savedInstanceState){ 
        super.onCreate(savedInstanceState); setContentView(R.layout.main);
    }
    long Current =0;
    //记录上次按下返回的时间
    public void onBackPressed(){
        if (System.currentTimeMillis()- Current >2000){
            Current=System.currentTimeMillis();
            Toast.makeText(this,"再点击一次推出",0).show();
        }else{
            finish();
            System.exit(0);
        }
    //如果当前时间大于上次记录的时间2秒，那么点一次时弹出Toast消息，如果小于2s,那么退出程序
    }
}
```
  
Tony-Chen 2018.3.16