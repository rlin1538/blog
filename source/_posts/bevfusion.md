---
title: 【BEV】BEVFusion——基于BEV表征的多任务多传感器融合
tags: 
    - BEV
    - 复现记录
categories:
    - 
date: 2023-11-09 10:00:00
comment: true
---

# 论文

论文的题目是：*[《BEVFusion: Multi-Task Multi-Sensor Fusion with Unified Bird’s-Eye View Representation》](https://arxiv.org/pdf/2205.13542.pdf)*

本文提出的BEVFusion是一种多任务多传感器融合框架，其统一BEV表征空间中的多模态特征，很好地保留了几何和语义信息。为实现这一点，优化BEV池化，诊断并解除视图转换中的关键效率瓶颈，将延迟减少了40倍。BEVFusion从根本上来说是任务无关的，无缝支持不同的3D感知任务，几乎没有架构的更改。

如图所示是BEVFusion流水线概览：给定不同的感知输入，首先应用特定于模态的编码器来提取其特征；将多模态特征转换为一个统一的BEV表征，其同时保留几何和语义信息；存在的视图转换效率瓶颈，可以通过预计算和间歇降低来加速BEV池化过程；然后，将基于卷积的BEV编码器应用到统一的BEV特征中，以缓解不同特征之间的局部偏准；最后，添加一些特定任务头支持不同的3D场景理解工作。![](2023-11-09-16-08-44.png)

本文的关键贡献在于，优化了BEV池化操作的效率。
* 预计算
* 间歇降低
![](2023-11-09-16-36-23.png)

通过以上优化，使得摄像头到BEV的转换速度提高了40倍！


# 代码

https://github.com/mit-han-lab/bevfusion

---
> # 参考
> https://zhuanlan.zhihu.com/p/521821929