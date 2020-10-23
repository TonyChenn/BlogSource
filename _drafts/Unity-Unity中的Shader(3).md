---
title: Unity-Unity中的Shader(3)
date: 2020-10-19 15:32:00
tags:
    - Unity
    - Shader
top:
password:
description:
img:
---
# UnityShader与材质
Unity Shader定义了渲染所需的各种代码（如片元着色器，顶点着色器），属性（如使用了哪些纹理）和指令（渲染和标签设置），而材质允许我们调节这些属性，并将最终结果赋给对应的模型。

# Unity中四种Shader模板
- Standard Surface Shader
包含了标准光照模型的表面着色器模板，是一种基于物理的着色器系统，以模拟真实的方式模拟材质和灯光的关系，可以轻松的表现出各种金属反光效果。
- Unlit Shader
基于顶点片段着色器，不受光照影响，包含雾效的基本顶点/片元着色器，多用于特效，UI特效的制作。
- Image Effect Shader
也是顶点片段着色器，只不过是为实现各种屏幕后处理效果提供的一个基本模板。
- Compute Shader
是运行在显卡上的一段程序，独立于常规渲染管线，可以直接将GPU作为并行处理器加以利用，使GPU不仅具备3D渲染能力，也有其他运算能力。

# Unity Shader结构
```
Shader "Custom/TestShader"
{
    Properties
    {
        //属性
    }
    SubShader
    {
        //显卡A的SubShader
    }
    SubShader
    {
        //显卡B的SubShader
    }
    FallBack "Diffuse"
    CustomEditor "EditorName"
}
```
## Shader名称
shader文件的第一行设置Shader的名字，如上面的Shader的名字是："Custom/TestShader".如果把路径名称放在Hidden下面的话，比如：
Shader "Hidden/TestShader"
则表示在材质面板中隐藏此Shader,将无法通过材质下拉列表中找到它。这在做一些不需暴露的Shader时很有用处，可以使Shader下拉列表更精简整洁。

## 属性(Properties)
Properties语句块的作用仅仅是为了让这些属性可以出现在材质面板上。
### Properties结构
- _Name 变量名
- display name 显示在材质面板上的名称
- PropertiesType 变量类型
- DefaultValue 默认值
```shader
Properties{
    _Name("display name",PropertiesType) = DefaultValue
}
```
### 属性类型
|属性类型|例子|
|---|---|
|Number相关||
|Int|_Int("Int", Int) = 1|
|Float|_Float("Float", Float) = 1.6|
|Range(min, max)|_Range("Range", Range(0.0, 1.0)) = 0.5|
|Vector,Color相关||
|Color|_Color("Color", Color) = (1,1,1,1)|
|Vector|_Vector("Vector", Vector) =(1, 2, 3, 4)|
|2D|_2D ("2D", 2D) = "white" {}|
|3D| _3D("3D", 3D) = "black" {}|
|Cube|_Cube("Cube",Cube) = "white"{}|
### 效果图
![UnityShaderProperties.jpg](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/1019/UnityShaderProperties.jpg)

## SubShader
每个Unity Shader最少包含1个SubShader块。当Unity需要加载当前Shader时会扫描所有的SubShader块，找到一个能在目标平台运行的SubShader,如果都不支持的话，会使用FallBack中指定的Unity Shader.
### SubShader结构
Subshader中定义了可选的Tags,RenderSetup(状态)，和一系列的Pass,每个Pass中都定义了一次完整的渲染流程，所以Pass数目过多会导致渲染性能下降，要减少使用Pass.
```shader
SubShader
{
    Tags { "RenderType"="Opaque" }
    Pass{
    }
}
```
#### Tags
SubShader的Tags是一个键值对，键和值都是字符串类型。这些键值对用来告诉Unity渲染引擎：SubShader希望怎样以及合适渲染这个对象。标签类型表如下：

- Queue 渲染队列，指定对象什么时候渲染，值有：
    1. Background（值为1000，此队列最先渲染）
    2. Geometry（值为2000，通常用于不透明物体）
    3. AlphaTest （值为2450）
    4. Transparent（值为3000，常用于半透明对象）
    5. Overlay（值为4000，常用于叠加效果）
- RenderType
- DisableBatching
- ForceNoShadowCasting
- IgnoreProjector
- CanUseSpriteAtlas 是否可用于精灵(Sprites)打包图集，bool类型
- PreviewType 指明在材质面板如何预览该材质。材质的默认预览时个球形，可以设置为其他类型，如"Plane","SkyBox"
#### RenderSetup
#### Pass块


## Fallback
跟在SubShader最后面。作用是当上面所有SubShader都不能在当前显卡运行的话，就运行这个Shader吧。当然课可以通过<b>Fallback off</b>关闭Fallback,意思是当所有SubShader都不能用时，就不管它了，直接跳过。

## CustomEditor
自定义界面