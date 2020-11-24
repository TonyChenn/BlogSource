---
title: Shader-新手引导遮罩
date: 2020-11-23 19:19:00
tags:
    - Unity
    - Shader
top:
password:
description:
img:
---
新手引导遮罩通常有两种实现方式。第一种：在UI最上层添加一层半透明遮罩，克隆要高亮的物体，放在遮罩上方。遮罩部分添加事件拦截如:(NGUI添加BoxClollider,UGUI，取消勾选RecastTarget)即可实现点击其他位置的事件拦截。第二种方式就是：使用shader实现遮罩效果。