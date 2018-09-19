---
layout: blog  
note: true  
title:  "Unity 学习笔记——关于动画重置的解决方法"  
tags:  
- Unity 学习笔记  
background: purple  
background-image: https://i.loli.net/2018/09/16/5b9df0987211a.jpg
date:   2018-09-16 14:02   
category: Unity 笔记
---

>在 Unity 动画系统中，动画播放完毕默认是停留在最后一帧的，当我们重置场景的时候，需要将动画重置到第一帧。而 Animation 和 Animator 组件中并没有 Reset 方法。本文主要提供了关于 Animation 和 Animator 动画重置的解决方法。

# Animator 重置到起始帧
通过 `Animator.Play` 和 `Animator.Update` 播放来进行控制。

## Animator.Play

* **函数原型**
```
void Play(string stateName, int layer = -1, float normalizedTime = float.NegativeInfinity);
```
* **参数列表**

Parameters 参数 | Description 描述
:-: | ---
stateName |The name of the state the will be played.<br>将要播放的动画状态名字。
layer | The layer where the state is.<br>动画状态所在的层。
normalizedTime | The normalized time at which the state will play.<br>将要播放动画状态的归一化时间。

## Animator.Update

* **函数原型**
```
void Update(float deltaTime);
```
* **参数列表**

Parameters 参数 | Description 描述
:-: | ---
deltaTime |The time delta. <br>增量时间。

## 示例代码
```
public void animToStart()
{
    //参数：动画名，层，时间
    animator.Play("Take 001", -1 ,0f);
    animator.Update(0f);
}
```

# Animaton 重置到起始帧

## 解决思路
先播放动画，经过一段极短的时间之后（当动画播完第一帧，还未开始播第二帧的时候），停止动画，从而使动画停留在第一帧的画面，达到动画重置的效果。

注 ：有时候帧率较低时，延时的时间过短可能不足以渲染一帧，导致该方法失效。经过测试，一般情况下，延时 0.03 - 0.05 秒后停止动画，能达到较好的效果。

## 示例代码

```
public void ResetScene()
{
    // Reset animations
    ani_1.Play();
    ani_2.Play();
    
    //Invoke(methodName: string, time: float): void;
    Invoke("stopAllAnimations", 0.05f);   
}

void stopAllAnimations()
{
    ani_1.Stop();
    ani_2.Stop();
}
```

## 参考资料
【1】http://wiki.ceeger.com/script:unityengine:classes:animator:animator.play  
【2】http://wiki.ceeger.com/script/unityengine/classes/animator/animator.update