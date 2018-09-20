---
layout: blog  
note: true  
title:  "Unity 学习笔记——如何使碰撞仅对特定物体有效"  
tags:  
- Unity 学习笔记  
icon: note  
background: red  
background-image: https://i.loli.net/2018/09/16/5b9df0987211a.jpg  
date:   2018-09-16 16:50   
category: Unity 笔记
---

> Unity 开发游戏时，我们希望 player 碰撞体是可以被墙壁遮挡，可以被敌人碰到消灭，但是多个 player 之间又是可以相互穿过，敌人之间也是可以互相穿过。所以问题来了，我们如何是碰撞仅对特定的物体有效？

## 解决方法
重点在于 `Physics` 和 `Layer` 的使用。

![吃豆人游戏](https://i.loli.net/2018/09/16/5b9e137e09c3d.png)

以《史上最难的游戏》的游戏为例，在游戏中，`红色的主角`有多个，由多人控制。`主角`可以与 `墙壁` 产生碰撞效果而被遮挡，也可以与蓝色的 `敌人` 产生碰撞效果而被消灭，但是多个 `主角` 之间可以叠加穿过而不产生碰撞。
### 1. 设置游戏物体 Layer
为了实现这种效果，首先我们要设置玩家对象、敌人的 Layer ，如 Player 和 Enemy 。
（在 Inspector 面板 Layer 中设置 ，如果没有，自行新建 Layer）

### 2. 设置 Physics 碰撞矩阵
找到菜单栏 `Edit` -> `Project Setting` -> `Physics` 并打开。

![Physics Setting](https://i.loli.net/2018/09/16/5b9e185b7b2a6.png)

我们展开 `Layer Collision Matrix` 部分，可以看到一个矩阵，这个矩阵描述了哪些 Layer 可以跟哪些 Layer 发生碰撞。如图，我们将不希望发生碰撞的 Layer 在矩阵中勾选取消即可。

![Collision Matrix](https://i.loli.net/2018/09/16/5b9e185b53d88.png)

## 参考资料
【1】https://jingyan.baidu.com/article/48b558e30d18947f38c09a2e.html