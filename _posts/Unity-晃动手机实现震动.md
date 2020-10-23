---
title: Unity-晃动手机实现震动
date: 2020-10-14 16:30:00
tags:
    - Unity
---

# 问题
手机摇一摇震动，然后执行晃动的监听方法

# 效果
![shake](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1014/shake.gif)

# 原理
Unity通过Input.acceleration 获取设备在三维空间中的线性加速度（调用了加速度传感器），线性加速度是一个矢量，在三维坐标系中可以分为x,y,z三个分量。当某个分量的值大于指定值后，判定设备进行晃动了，执行晃动方法。

# 脚本
```Csharp
/// x坐标晃动监听
public class Test : MonoBehaviour
{
    [SerializeField] Button BtnShake;
    [SerializeField] Text Tip;

    float oldY = 0;
    float curY = 0;
    float offset = 0;
    //在Unity中震动强度为[0，1]
    float shakeStrength=.5f;

    private void Start()
    {
        BtnShake.onClick.AddListener(() =>
        {
            Handheld.Vibrate();
        });
    }
    void Update()
    {
        curY = Input.acceleration.y;
        offset = curY - oldY;
        oldY = curY;

        if(offset > shakeStrength)
        {
            Handheld.Vibrate();
            Tip.text = "晃动了";
            //TODO
        }
    }
}
```
