---
title: Unity-Unity与Android交互
date: 2019-04-26 19:06:54
tags: Unity
description: AndroidStudio制作并导出aar包，在Unity中使用
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0426/icon.png
---
# 前言
用到Unity与Android好多次了，每次都需要重复的找资料，造轮子，这次接了一个小项目后又要用到，完成后下定决心记录AndroidStudio打aar包的过程，本来还想封装起来以后直接用，后来发现这个做法不可行。那以后还是老老实实用一个做一个吧。


# 过程
1. Unity端创建项目，切换为Android平台，修改包名，我这里设置为"com.dt.test"

2. 创建AndroidStudio工程，创建Empty Activity,名称随意，包名随意，最低API也根据自己安装的Android SDK选择。选择Android Library，创建模块。
   
   ![au1](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0426/au1.jpg)

   ![newmodule](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0426/newmodule.jpg)

3. 在模块中创建EmptyActivity,勾选<bold>Launcher Activity</bold>

4. 从Unity安装路径下找到classes.jar包导入并添加到模块的libs文件夹下
```
   Windows: ...unity\Editor\Data\PlaybackEngines\AndroidPlayer\Variations\mono\Release\Classes\classes.jar
   Mac:...unity/PlaybackEngines/AndroidPlayer/Variations/mono/Release/Classes/classes.jar
```
5. 删除activity_main.xml，在MainActivity.java中删除```setContentView(R.layout.activity_main);```，并让MainActivity继承UnityPlayerActivity。

6.将app包下的manifest的application标签中的内容覆盖掉Unity_Android_Library的application的标签中内容，删除爆红的三行，在```</intent-filter>```标签结尾后添加新的标签```<meta-data android:name="unityplayer.UnityActivity" android:value="true"/>```

7. 在MainActivity.java 中书写需要用到的方法，举几个常用方法：
```java
//Toast
public void ShowToast(String msg){
    Toast.makeText(getApplicationContext(),msg,Toast.LENGTH_LONG).show();
}

//离开对话框
public static void ShowDialogSync(){
    UnityPlayer.currentActivity.runOnUiThread(new Runnable() {
        @Override
        public void run() {
            AlertDialog.Builder builder=new AlertDialog.Builder(UnityPlayer.currentActivity);
            builder.setTitle("提示");
            builder.setMessage("你确定要退出吗？");
            //builder.setIcon(R.drawable.iconic);
            builder.setPositiveButton("取消", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialogInterface, int i) {
                    dialogInterface.dismiss();
                }
            });
            builder.setNegativeButton("确定", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialogInterface, int i) {
                    System.exit(0);
                }
            });
            builder.show();
        }
    });
}

//打开QQ
public void OpenQQSync(final String qq){
    UnityPlayer.currentActivity.runOnUiThread(new Runnable() {
        @Override
        public void run() {
            String url = "mqqwpa://im/chat?chat_type=wpa&uin="+qq+"&version=1";
            try {
                startActivity(new Intent("android.intent.action.VIEW", parse(url)));
            } catch (Exception e) {
                ShowToast("本机未安装QQ");
            }
        }
    });

}

//打开URL
public void OpenUrlSync(final String url){
    UnityPlayer.currentActivity.runOnUiThread(new Runnable() {
        @Override
        public void run() {
            try {
                startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse(url)));
            }catch (Exception ex){
                ShowToast(ex.toString());
            }
        }
    });
}
```

8. 选中模块 ,点击Bulid/Make select Modules，导出aar包，并复制到桌面，将minifest文件也复制到桌面。

9. 使用压缩工具打开aar包，删除libs\下的classes.jar

10. 导入Unity Assets\Plugins\Android路径下

11. 封装了一个类，供以后使用
```csharp
using UnityEngine;

public class AndroidUtils
{
    static AndroidJavaClass jc;
    static AndroidJavaObject jo;
    static AndroidUtils _instance = null;

    private AndroidUtils()
    {
        jc = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
        jo = jc.GetStatic<AndroidJavaObject>("currentActivity");
    }
    public static AndroidUtils Instance
    {
        get
        {
            if (_instance == null) _instance = new AndroidUtils();

            return _instance;
        }
    }

    public void ShowDialog()
    {
        jo.CallStatic("ShowDialogSync");
    }

    public void ShowToast(string str)
    {
        jo.Call("ShowToast", str);
    }

    public void OpenQQ(string qq)
    {
        jo.Call("OpenQQSync", qq);
    }

    public void OpenUrl(string url)
    {
        jo.Call("OpenUrlSync", url);
    }
}
```

12. 调用
```csharp
Toast.onClick.AddListener(() =>
{
    AndroidUtils.Instance.ShowToast("Toast");
});

Dialog.onClick.AddListener(() =>
{
    AndroidUtils.Instance.ShowDialog();
});

OpenQQ.onClick.AddListener(() =>
{
    AndroidUtils.Instance.OpenQQ("xxx");
});

URL.onClick.AddListener(() =>
{
    AndroidUtils.Instance.OpenUrl("https://www.baidu.com");
});
```
# 效果
![result](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2019/0426/result.jpg)