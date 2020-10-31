---
title: NGUI-UIScrollView剪切自定义Shader
date: 2020-08-20 14:28:00
updated: 
tags: 
    - Unity
    - NGUI
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0820/unclip.gif
description: UIScrollView剪切自定义Shader
top: 
---
之前使用NGUI做一个卡牌合成模块时，要求合成新的卡牌后要出现闪耀的特效3下，由于有许多卡牌的，所以要把卡牌放到UIScrollView中。理论没有问题，但是实际开发过程中遇到的两个问题，下面记录下解决方案。

# 在UIScrollView中使用Shader没效果
原因是渲染时创建Material时找不到自定义的Shader导致的，看下面<kbd>UIDrawCall.cs</kbd>中截取的部分代码：
```csharp
void CreateMaterial ()
{
    ///
    ///...省略
    ///

    //从这里开始就是查找Shader的过程
    //此处为Clipping为TextureMask的情况（咱们使用的时SoftClip模式）
    if (panel != null && panel.clipping == Clipping.TextureMask)
    {
        mTextureClip = true;
        shader = Shader.Find("Hidden/" + shaderName + textureClip);
    }
    //mClipCount大于0
    else if (mClipCount != 0)
    {
        //这里找不到
        shader = Shader.Find("Hidden/" + shaderName + " " + mClipCount);
        if (shader == null) shader = Shader.Find(shaderName + " " + mClipCount);

        // Legacy functionality
        if (shader == null && mClipCount == 1)
        {
            mLegacyShader = true;
            shader = Shader.Find(shaderName + soft);
        }
    }
    //这里走不到
    else shader = Shader.Find(shaderName);

    //添加：当找不到shader时，直接查找shader名
    if (shader == null) shader = Shader.Find(shaderName);

    //正常情况下使用自定义的shader就找不到shader了
    // NGUI默认指定：如果找不到shader时，使用的默认shader
    if (shader == null) shader = Shader.Find("Unlit/Transparent Colored");
    
    ///
    ///...省略
    ///
}
```

# 在UIScrollView中使用Shader滚动时不被剪切
上面的问题刚解决就又出现新问题了，UIScrollView中的Item上使用Shader时，无法被UIScrollView剪裁。如下所示：
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0820/unclip.gif)
下面记录下解决方案。