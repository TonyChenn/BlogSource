---
title: Unity-NGUI物体扫光特效
date: 2020-05-14 13:55:42
tags: 
    - Unity
    - Shader
img:
description: NGUI下物体扫光特效
top:
---
最近搞了多张卡牌合成新卡牌后，新卡牌在放回背包后闪亮一下，没搞过Shader,但前人已经造好轮子，研究后，记录下，方便以后使用
# 效果预览
![Previous](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0514/LightWalk.gif)


# Shader如下
```shader
Shader "Unlit/Walk light"
{
	Properties
	{
		_MainTex ("Base (RGB), Alpha (A)", 2D) = "black" {}
		_LightTex ("Light", 2D) = "black" {}
	}
	
	SubShader
	{
		LOD 200

		Tags
		{
			"Queue" = "Transparent"
			"IgnoreProjector" = "True"
			"RenderType" = "Transparent"
		}
		
		Pass
		{
			Cull Off
			Lighting Off
			ZWrite Off
			Fog { Mode Off }
			Offset -1, -1
			Blend SrcAlpha OneMinusSrcAlpha

			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag			
			#include "UnityCG.cginc"

			sampler2D _MainTex;
			sampler2D _LightTex;
	
			struct appdata_t
			{
				float4 vertex : POSITION;
				float2 texcoord : TEXCOORD0;
				fixed4 color : COLOR;
			};
	
			struct v2f
			{
				float4 vertex : SV_POSITION;
				half2 texcoord : TEXCOORD0;
				fixed4 color : COLOR;
			};
	
			v2f o;

			v2f vert (appdata_t v)
			{
				o.vertex = UnityObjectToClipPos(v.vertex);
				o.texcoord = v.texcoord;
				o.color = v.color;
				return o;
			}
				
			fixed4 frag (v2f IN) : COLOR
			{
				fixed4 main = tex2D(_MainTex, IN.texcoord);
				
                half lightU = IN.texcoord.x - frac(_Time.y);
                half2 lightUV = half2(lightU, IN.texcoord.y);
                fixed4 light = tex2D(_LightTex, lightUV);
				
				fixed4 col = main + main * light.a;
				return  col * IN.color;
			}
			ENDCG
		}
	}

	SubShader
	{
		LOD 100

		Tags
		{
			"Queue" = "Transparent"
			"IgnoreProjector" = "True"
			"RenderType" = "Transparent"
		}
		
		Pass
		{
			Cull Off
			Lighting Off
			ZWrite Off
			Fog { Mode Off }
			Offset -1, -1
			ColorMask RGB
			Blend SrcAlpha OneMinusSrcAlpha
			ColorMaterial AmbientAndDiffuse
			
			SetTexture [_MainTex]
			{
				Combine Texture * Primary
			}
		}
	}
}

```

# 使用
1. 新建一个Material,将Material的Shader选择为上面添加的Shader,如下图，看看就明白怎么设置：
![MaterialSetting](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0514/MaterialSetting.jpg)
2. 新建UITexture,把上面创建的Material拖拽赋值给UITexture;
![TextureSetting.jpg](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0514/TextureSetting.jpg)
3. 运行预览</br>
![previous.gif](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2020/0514/previous.gif)

代码控制就不写了，网上一大把。自己想想也能明白。

# 在NGUI的UIScrollView下特效不显示解决
原因是在NGUI的UIScrollView中被剪裁掉了，只需要选择Clipping为：**Constrain But Dont Clip**即可。