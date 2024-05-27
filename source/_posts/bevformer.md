---
title: 【BEV】BEVFormer——利用时空 Transformer 从多相机图像中学习鸟瞰图表示
tags: 
    - BEV
    - 复现记录
categories:
    - 
date: 2024-04-16 16:00:00
comment: true
---

# 论文


论文的题目是：*[《BEVFormer: Learning Bird's-Eye-View Representation from Multi-Camera Images via Spatiotemporal Transformers》](https://arxiv.org/abs/2203.17270)*

# 复现

## 环境安装

> 官方教程：https://github.com/fundamentalvision/BEVFormer/blob/master/docs/install.md


1. 创建conda环境

``` bash
conda create -n open-mmlab python=3.8 -y
conda activate open-mmlab
```

2. 安装pytorch

``` bash
pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 torchaudio==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html
# Recommended torch>=1.9

```

3. 安装gcc6

``` bash
conda install -c omgarcia gcc-6 # gcc-6.2
```

4. 安装mmcv-full、mmdet、mmseg

``` bash
pip install mmcv-full==1.4.0
#  pip install mmcv-full==1.4.0 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html
pip install mmdet==2.14.0
pip install mmsegmentation==0.14.1
```
5. 安装mmdet3d
``` bash
git clone https://github.com/open-mmlab/mmdetection3d.git
cd mmdetection3d
git checkout v0.17.1 # Other versions may not be compatible.
pip install -e .
```
6. 安装detectron2和Timm
``` bash
pip install einops fvcore seaborn iopath==0.1.9 timm==0.6.13  typing-extensions==4.5.0 pylint ipython==8.12  numpy==1.19.5 matplotlib==3.5.2 numba==0.48.0 pandas==1.4.4 scikit-image==0.19.3 setuptools==59.5.0
python -m pip install 'git+https://github.com/facebookresearch/detectron2.git'
```

7. 下载BEVFormer
``` bash
git clone https://github.com/fundamentalvision/BEVFormer.git
cd bevformer
mkdir ckpts
cd ckpts & wget https://github.com/zhiqi-li/storage/releases/download/v1.0/r101_dcn_fcos3d_pretrain.pth
```