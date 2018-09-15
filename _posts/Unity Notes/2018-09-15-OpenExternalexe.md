---
layout: blog  
note: true  
title:  "Unity 学习笔记——如何打开外部 exe 程序"  
tags:  
- 灵感记录  
- Unity 学习笔记  
background: red  
background-image: https://i.loli.net/2018/09/15/5b9d23e035119.jpg
date:   2018-09-15 22:49   
category: Unity 笔记
---
>问题描述：我们需要用 Unity 制作一款桌面应用，通过它可以方便地管理外部的一系列程序，所以，如何在Unity中打开外部exe程序是我们面对的第一个问题。

## 解决方法

**启动外部程序时**：直接使用` Process.Start();` 来启动外部程序，参数为 `需要启动的外部程序所在文件位置`。

**关闭外部程序时**：使用 `process.Kill();` 来关闭外部程序。

#### 示例代码
```
using UnityEngine;
using System.Collections;
using System.Diagnostics;//用于允许使用(Process)

public class example : MonoBehaviour
{
    public Rect windowRect = new Rect(20, 20, 120, 50);
    void OnGUI()
    {
        if (GUI.Button(new Rect(10, 20, 100, 20), "QQ"))
        {
            // 直接打开外部文件QQ等 exe</font>
            Process.Start("C:\\Program Files (x86)\\Tencent\\QQ\\Bin\\QQ.exe");
        }
    }
}
```

## 参考资料

[http://www.manew.com/blog-19044-735.html](http://www.manew.com/blog-19044-735.html)