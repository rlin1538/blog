title: Livox Mid-70 入门使用
tags: 
    - 激光雷达
categories:
    - 
date: 2023-12-13 19:00:00
comment: true
---

# 简介

Livox Mid-70是一款高性价比、安全可靠的激光探测测距仪传感器，可广泛应用于包括无人驾驶、工业搬运机器人、室内/室外服务机器人、特种机器人等众多领域，例如AGV、AMR、自动叉车、医疗后勤机器人、清洁机器人、末端配送机器人、智能安防机器人等。Livox Mid-70最小探测距离为0.05m,最大探测距离可达260 m。

![](2023-12-13-17-03-39.png)

> [Mid-70 用户手册](https://terra-1-g.djicdn.com/65c028cd298f4669a7f0e40e50ba1131/Download/Mid-70/new/Livox%20Mid-70%20User%20Manual_CHS_v1.2.pdf)

# 环境搭建

> Livox官方提供了多平台（Windows和Ubuntu）的支持，为接近实际开发场景，一般选用Ubuntu18.04来搭建环境，Windows上也有Livox viewer可以方便查看激光雷达点云信息。

为图省事，没安装双系统，尝试在win11的wsl2平台上提供的Ubuntu18.04子系统来搭建环境。

## 系统安装

参考官方提供的文档直接安装：https://learn.microsoft.com/zh-cn/windows/wsl/install

然后到微软商城搜索Ubuntu，选择18.04的版本直接安装。

> 注意wsl的版本，后续需要用到图形化界面的功能，只有wsl2能够使用。

## 环境配置

### ROS环境

ROS（英语：Robot Operating System，一般译为机器人操作系统），是专为机器人软件开发所设计出来的一套电脑操作系统架构。

目前重点不是这个，所以不做过多介绍，ros环境是许多项目的基础环境，所以需要配置好。

这里直接用一键安装的脚本，可以非常非常方便的一键安装所需环境。

``` bash
wget http://fishros.com/install -O fishros && . fishros
```
一路按推荐选项即可。
选择的是ROS1的环境，暂时不需要ROS2。
>
### Livox-SKD

#### 依赖

Livox SDK 依赖于 cmake 。你可以通过 apt 工具安装这些依赖包 :

``` bash
sudo apt install cmake
```

#### 编译 Livox SDK
在 Livox SDK 目录中，执行以下指令编译工程:

``` bash
git clone https://github.com/Livox-SDK/Livox-SDK.git
cd Livox-SDK
cd build && cmake ..
make
sudo make install
```
> https://github.com/Livox-SDK/Livox-SDK/blob/master/README_CN.md

### Livox ROS 驱动

