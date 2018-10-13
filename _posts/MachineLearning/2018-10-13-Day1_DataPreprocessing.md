---
layout: blog  
note: true  
title:  "机器学习入门笔记【Day 1】— 数据预处理"  
tags:  
- 机器学习  
- python  
background: green  
background-image: http://pf6qvqv35.bkt.clouddn.com/coverpage/MLcoverpage.jpg  
date:   2018-10-13 17:00   
category: 机器学习
---

## 第一步：导入需要的库
这两个是我们每次都需要导入的库
* Numpy ：包含数学计算函数
* Pandas ：用于导入和管理数据集


```python
import numpy as np
import pandas as pd
```

## 第二步：导入数据集
数据集通常为 `.csv` 格式。 CSV 文件以文本形式保存表格数据。文件的每一行是一条数据记录。我们使用 Pandas 的 read_csv 方法读取本地的 csv 文件为一个数据帧。然后，从数据帧中制作自变量和因变量的矩阵和向量。


```python
dataset = pd.read_csv('Data.csv')

# iloc 表示取数据集中的某些行和某些列，逗号前表示行，逗号后表示列
# 这里表示取所有行，x 取除最后有一列的所有列，y 取最后一列
x = dataset.iloc[ : , :-1].values
y = dataset.iloc[ : , 3].values
```

## 第三步：处理丢失数据
我们得到的数据很少是完整的。数据可能因为各种原因丢失，为了不降低机器学习模型的性能，需要处理数据。我们可以用整列的平均值，或中间值替换丢失的数据。我们用 `sklearn.preprocessing` 库中的 `Imputer` 类完成这项任务。
详情参考：[sklearn.preprocessing.Imputer](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Imputer.html)


```python
from sklearn.preprocessing import Imputer

imputer = Imputer(missing_values = "NaN", strategy = "mean", axis = 0)
imputer = imputer.fit(x[ : , 1:3])
x[ : , 1:3] = imputer.transform(x[ : , 1:3])
```

## 第四步：解析分类数据
分类数据指的是含有标签值而不是数字值的变量。取值范围通常是固定的。例如“Yes”或“No”不能用于模型的数学计算，所以需要解析成数字。为了实现这一功能，我们从 `sklearn.preprocessing` 库导入 `LabelEncoder` 类。
详情参考：[sklearn.preprocessing.LabelEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html) , 
[sklearn.preprocessing.OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)


```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
labelEncoder_x = LabelEncoder()
x[ : , 0] = labelEncoder_x.fit_transform(x[ : , 0])
```

**创建虚拟变量**


```python
onehotencoder = OneHotEncoder(categorical_features=[0])
x = onehotencoder.fit_transform(x).toarray()
labelEncoder_y = LabelEncoder()
y = labelEncoder_y.fit_transform(y)
```

## 第五步：拆分数据集为测试集合和训练集合
把数据集拆分成两个：一个用来训练模型的训练集合，另一个用来验证模型的测试集合。二者比例一般是 80 ：20。我们导入 `sklearn.model_selection` 库中的 `train_test_split()` 方法。详情参考：[sklearn.model_selection.train_test_split](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)


```python
from sklearn.model_selection import train_test_split
x_train,x_test,y_train,y_test = train_test_split(x,y,test_size = 0.2, random_state = 0)
```

## 第六步：特征缩放
大部分模型算法使用两点之间的欧式距离表示，但是此特征在幅度、单位和范围姿态问题上变化很大。在距离计算中，高幅度的特征比低幅度的特征权重更大。可以用特征标准化或 Z 值归一化解决。导入 `sklearn.preprocessing` 库的 `StandardScaler` 类。
详情参考：[sklearn.preprocessing.StandardScaler](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)


```python
from sklearn.preprocessing import StandardScaler
sc_x = StandardScaler()
x_train = sc_x.fit_transform(x_train)
x_test = sc_x.transform(x_test)
```
