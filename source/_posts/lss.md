---
title: 【BEV】BEV模型开山论文——Lift-Splat-Shoot
tags: 
    - BEV
    - 复现记录
date: 2023-10-23 10:00:00
comment: true
---

# 论文

论文的题目是：*《[LSS: Lift, Splat, Shoot: Encoding Images from Arbitrary Camera Rigs by Implicitly Unprojecting》](https://arxiv.org/pdf/2008.05711.pdf)*

其核心思想是通过显示估计图像的深度信息，对采集到的环视图像进行特征提取，根据估计出来的离散深度信息，实现图像特征向BEV特征的转换，进而完成自动驾驶中的语义分割任务。

### Lift

提取图像特征，然后将每个图片从二维提升到统一的三维坐标系下，得到像素点在3D空间中的特征，作者将每个像素点都生成所有可能深度，得到一组离散化的深度空间，对应一个棱台状的空间点云分布![](2023-10-23-15-46-57.png)

### Splat

将Lift得到的点云转化成Pillars（无限高的体素），就是将点分配到离他最近的Pillars中，然后求和池化得到一个C\*H\*W的张量，对这个张量进行CNN可以得到鸟瞰图的预测结果![](2023-10-23-15-54-28.png)

### Shoot

对splat得到的特征进行编解码处理，可以看作是bev特征提取器，将编解码后的特征用于目标任务

# 代码

学习链接：[手撕BEV的开山之作](https://www.bilibili.com/video/BV16T411g7Gc)



# 复现

https://github.com/nv-tlabs/lift-splat-shoot

首先需要用到nuscenes数据集（https://www.nuscenes.org），下载mini的完整数据集以及map extension 1.3

然后下载预训练的模型： https://drive.google.com/file/d/18fy-6beTFTZx5SrYLs9Xk7cY-fGSm7kw/view?usp=sharing

然后可以运行如下代码生成鸟瞰图：

``` python
python main.py viz_model_preds mini --model
f=E:\Download\model525000.pt --dataroot=F:\Datasets\Nuscenes-mini --map_folder=F:\Datasets\Nuscenes-mini

# 其中--modelf为预训练模型目录，--dataroot为nuscenes数据集根目录，--map_folder为map extension根目录
```
