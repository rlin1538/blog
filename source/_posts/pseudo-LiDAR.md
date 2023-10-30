---
title: 【BEV】伪雷达点云—Pseudo-LiDAR
tags: 
    - BEV
    - 激光雷达
date: 2023-10-30 15:35:00
comment: true
---

# 论文

论文的题目是：*《[Pseudo-LiDAR from Visual Depth Estimation: Bridging the Gap in 3D Object Detection for Autonomous Driving》](https://arxiv.org/abs/1812.07179)*

论文探讨了基于图像的3D感知和基于LiDAR的3D感知之间的差异，指出基于图像方法的不足，提出了Pseudo-LiDAR来从图像建立点云数据进行感知任务。

### Image-based 3D Perception 劣势原因

* 不准确的深度信息

* 图像2D的表示方式

后者是造成2维卷积在基于图像的感知任务中使图像扭曲的原因。作者分析了两个主要问题：一是不同物体间应该不连贯，但2D图像中不同深度的物体均在一个平面上（不像点云那种，不同深度不连贯）；二是前后景不同尺度，2D图像远处物体由于透视原因尺度较小，检测成为一个较难的问题（雷达点云则能保持三维空间中原始的尺度）。

### Pseudo-LiDAR 流程

![](2023-10-30-16-00-41.png)

其首先利用图像深度预测模型（如DRON、PSMNET）从单目或立体图像获取对应深度图像；然后将原图像和深度信息结合得到伪雷达点云，将像素反投影到3D坐标中，得到点云；最后用伪点云代替原始雷达点云，在基于点云的感知模型中进行感知任务（如F-PointNet、AVOD）。

# 代码

https://github.com/mileyan/pseudo_lidar