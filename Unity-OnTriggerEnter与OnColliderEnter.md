---
title: Unity学习笔记
date: 2018-01-05 13:01:20
tags: Unity
---
![](https://ws1.sinaimg.cn/mw690/006PThdlgy1fw9uwkmua7j30jf0cq4aq.jpg)
<!--more-->
# 1.OnTriggerEnter与OnColliderEnter

1）如果想实现两个刚体物理的实际碰撞效果时候用OnCollisionEnter，Unity引擎会自动处理刚体碰撞的效果。OnCollisionEnter方法必须是在两个碰撞物体都不勾选isTrigger的前提下才能进入。  
2）如果想在两个物体碰撞后自己处理碰撞事件用OnTriggerEnter。只要勾选一个isTrigger那么就能进入OnTriggerEnter方法。


# 2.Object跟随鼠标移动而旋转 
 ```csharp
private float minXRotation = -40;
private float maxXRotation = 40;
private float minYRotation = -60;
private float maxYRotation = 60;

private void Update()
{
    float xPosPrecent = Input.mousePosition.x / Screen.width;
    float yPosPrecent = Input.mousePosition.y / Screen.height;

    float xAngle = -Mathf.Clamp(yPosPrecent * maxXRotation, minXRotation, maxXRotation);
    float yAngle = Mathf.Clamp(xPosPrecent * maxYRotation, minYRotation, maxYRotation);
    this.gameObject.transform.eulerAngles=new Vector3(xAngle, yAngle, 0);
}
```
**Mathf.Clamp(value,min,max);**

当旋转角度大于max，value=max;

当旋转角度小于min，value=min;

else value=value;  


 Tony-Chen   
 **留人间多少爱，迎浮世千重变，与有情人做快乐事，别问是劫是缘。**