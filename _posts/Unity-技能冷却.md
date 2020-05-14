---
title: Unity-技能冷却
date: 2017-11-30 11:48:01
tags: Unity
description: 技能冷却特效
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.30/icon.jpg
---

国际惯例：先放效果图：   

![show](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.30/show.jpg)

 那~就实现这个效果，当点击技能按钮后，技能进入冷却状态，技能逐渐冷却完成，相信经常玩游戏的都知道；

 结构图：   
![struct](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/11.30/struct.jpg)

 挺简单，解释在图片上；   
 更改FillImage上的ImageType属性为:Failed;

 下面实现由代码控制每次点击按钮进入冷却状态，2秒后冷却完成；

 
```csharp
public class Skill : MonoBehaviour {
    //冷却时间
    public float ColdTime = 2f;
    //计时器
    float timer = 0;
    //是否开始冷却
    bool startTimer = false;
    //冷却图片
    private Image fillImage;
    void Start () {
        //获得冷却图片
        fillImage = transform.Find("FillImage").GetComponent<Image>();
    }

    void Update () {
        if (startTimer)
        {
            timer += Time.deltaTime;
            //设置冷却图片显示比率
            fillImage.fillAmount = (ColdTime - timer) / ColdTime;
            if(timer>=ColdTime)
            {
                fillImage.fillAmount = 0;
                timer = 0;
                startTimer = false;
            }
        }
    }

    public void OnReleaseSkillClickListener()
    {
        //点击按钮开始冷却
        startTimer = true;
    }
}

```
 Tony-Chen   
 2017.11.30

 **这个世界很美好，值得我们为之去奋斗。我只认同后半句。**