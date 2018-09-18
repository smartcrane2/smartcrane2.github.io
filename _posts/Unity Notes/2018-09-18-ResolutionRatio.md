---
layout: blog  
note: true  
title:  "Unity 学习笔记——如何解决打包时候分辨率失效的问题"  
tags:  
- Unity 学习笔记  
background: orange  
background-image: https://i.loli.net/2018/09/16/5b9df0987211a.jpg  
date:   2018-09-18 22:50   
category: Unity 笔记
---

>在unity完成一个自定义分辨率的游戏，打包成 PC 版本时，经常会遇到一个问题，就是明明已经在 build setting 中设定了分辨率，为什么打包后运行时却失效了？本文将讲解如何解决打包时分辨率失效的问题。

## 为什么分辨率会重置？
当你用 `Unity` 构建一个新的游戏时，他会为你在 `Player Settings` 的公司名称下保存一个 `注册表键（windows 中）` 或 `preference file（ Mac 上）`。

默认情况下，这是 `DefaultCompany`。在第一次打包时，Unity 会自动像注册表中注册打包信息，后续打包时，如果公司名称没有变的话，`注册表键` 或 `preference  file` 还包含之前显示设置和其他设置，不会覆盖当前你新构建的设置，也就是说下次打包即使重新设置分辨率也没用。

![image](http://pf6qvqv35.bkt.clouddn.com/talk/20180918/20180907112244734.png)

## 如何解决
### 方法一：修改项目名称
修改项目名称（Product Name），重新打包。（这种方法比较草率，治标不治本）

![image](http://pf6qvqv35.bkt.clouddn.com/playersettings.png)

### 方法二：修改注册表
* 按 `Win + R` ，在运行窗口输入 `regedit` ，回车打开注册表。

![image](http://pf6qvqv35.bkt.clouddn.com/talk/20180918/run.png)

* Unity 注册的位置在 `HKEY_CURRENT_USER` \ `Software` \ `（公司名称）` \ `（项目名称）` 。

![image](http://pf6qvqv35.bkt.clouddn.com/talk/20180918/regedit.png)

* 修改分辨率的高和宽，下次打包即可生效。

### 方法三：
此外还可以用代码来控制（大招，最终解决方案），在加载的第一个场景中的 C# 脚本中，`Start()` 函数中加入以下代码即可。

```
void Start()
{
    // width 和 height 为分辨率的宽和高
    Screen.SetResolution(width,height,true);
}
```

## 参考资料
【1】[Unity打包PC分辨率不能修改的问题 -- CSDN](https://blog.csdn.net/gqj108/article/details/82493828)  
【2】[Unity3d 发布EXE后分辨率的问题处理 -- CSDN](https://blog.csdn.net/htwzl/article/details/79550541)  
【3】[Unity发布自定义分辨率程序 -- CSDN](https://blog.csdn.net/qq527703883/article/details/78064867)