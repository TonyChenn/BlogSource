---
title: Unity-DOTween插件的使用
date: 2018-04-08 20:21:15
tags: Unity
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.08/icon.jpg
---

# 1.添加插件，添加命名空间
```
using DG.Tweening;
```
| 问题 | 解决方案 | 
| :------| :------ |
| 创建动画后，动画会自动播放 | tweener.Pause(); |
| 播放完成后，动画默认自动销毁 | tweener.SetAutoKill(false); |

## 值的渐变
```csharp
DoTween.To(()=>current,x=>current=x,target,time);
```

## Simple(简单方式)：

从当前位置经过time秒后移动到vector3的位置
使用Tweener对象得到动画
```csharp
Tweener tweener=transform.DOMove(vector3,time);	
Tweener tweener=transform.DOLocalMove(vector3,time);
tweener.SetAutoKill(false);			//不让动画自动销毁
//正序播放
transform.DoPlayForward();
//倒放动画
transform.DOPlayBackwards();
//播放
tweener.Play();
//暂停
tweener.Pause();
```



## DoTween---FromTween
```csharp
transform.DOMoveX(5,1);				//从当前位置经过1秒后移动到5的位置
transform.DOMoveX(5,1).from();		        //从目标位置移动到当前位置
transform.DOMoveX(5,1).from(true);		//从目标位置移动5个单位
```


## 动画属性
//Ease是一个枚举类型
```csharp
Tweener tweener=transform.DOMoveX(0,1);
tweener.SetEase(Ease.InBack);				//设置曲线
tweener.SetLoop(num);					//循环播放num次;
tweener.OnComplete(Methord);				//动画播放完成，执行Methord()事件;
```

## 对话框，文字动画
```csharp
Private Text text;
text.DOText(str_target,time);					//从空白经过time秒后逐渐显示出来
```




## 屏幕震动
//原理：对Camera进行操作(使其坐标随机移动)
```csharp
transform.DOShakePosition(time,float num);		        //num范围0-1
transform.DOShakePosition(time,new Vector3(1,1,0));		//只在x,y轴震动
```


颜色和透明度
```csharp
Text text;
text.DOColor(targetColor,time);		        //从当前颜色经过time秒后变成targetColor
text.DOFade(alpha,time);			//从当前透明度经过time秒后变成alpha的透明度
```





//可视化动画编辑
//控件上添加组件 DOTweenAnimation
//代码控制DOTweenAnimation上创建的动画
```csharp
DoTweenAnimation dta;
dta.Play();						//播放
dta.DoPlayForward();
```


//路径编辑器

//控件上添加组件 DOTweenPath


更多：访问DOTween官网：http://dotween.demigiant.com


```
2018.4.8
TonyChen