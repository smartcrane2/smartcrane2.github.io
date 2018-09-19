---
layout: blog  
note: true  
title:  "Unity 学习笔记——点光源个数较多时部分光源失效的问题"  
tags:  
- Unity 学习笔记  
background: yellow  
background-image: https://i.loli.net/2018/09/16/5b9df0987211a.jpg  
date:   2018-09-19 14:40   
category: Unity 笔记
---

>在 Unity 中，当场景中布置的 `点光源` 个数较多时，会发现有部分光源失效。其实，这是由于 Unity 里 `Quality Setting` 中的相关限制导致的。本文主要讲解如何解决点光源较多时部分点光源失效的问题。

## 解决方案

* 点击主菜单 `Edit` -> `Project Settings` -> `Quality` ，打开 
`QualitySettings` 窗口。

![image](http://pf6qvqv35.bkt.clouddn.com/note/20180919/lightSetting.png)

* 在 `Rendering` 选项下找到 `Pixel Light Count`，这即是场景中渲染的点光源个数，其默认值是 4 ，将其改为自己想要的值即可。

![image](http://pf6qvqv35.bkt.clouddn.com/note/20180919/QualitySettings.png)

## 参考资料
【1】https://blog.csdn.net/tinyhum3d/article/details/17076469