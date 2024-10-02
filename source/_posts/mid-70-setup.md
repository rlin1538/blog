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

### 外部电源设计

采用现成的18650锂电池组（某宝入手，3000毫安时的五十多块钱），带DC公头和母头。mid-70的电源线有正负极接线，安装带附送的DC母头上，插到电池组上完成供电。![](2023-12-19-14-23-46.png)

> [Mid-70 用户手册](https://terra-1-g.djicdn.com/65c028cd298f4669a7f0e40e50ba1131/Download/Mid-70/new/Livox%20Mid-70%20User%20Manual_CHS_v1.2.pdf)

# 环境搭建

> Livox官方提供了多平台（Windows和Ubuntu）的支持，为接近实际开发场景，一般选用Ubuntu18.04来搭建环境，Windows上也有Livox viewer可以方便查看激光雷达点云信息。

为图省事，没安装双系统，尝试在win11的wsl2平台上提供的Ubuntu18.04子系统来搭建环境。

## 系统安装

参考官方提供的文档直接安装：https://learn.microsoft.com/zh-cn/windows/wsl/install

然后到微软商城搜索Ubuntu，选择18.04的版本直接安装。

> 注意wsl的版本，后续需要用到图形化界面的功能，只有wsl2能够使用。
> ubuntu20.04 也能搭环境，但后面跑一些算法时需要额外修改一些代码。
> 一开始我用了18.04，但是GUI太卡了，点云一多，卡到爆炸，后面查了一下，18.04似乎不支持GUI显卡加速。所以改用20.04

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

安装完成后执行`roscore`，如没报错则完成ROS环境搭建，如提示不存在命令，则需更新一下source ROS目录下的setup.sh

``` bash
source /opt/ros/noetic/setup.sh
echo "source /opt/ros/noetic/setup.sh" >> ~/.bashrc
source ~/.bashrc

## noetic 为你的ROS版本名称
```
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

1. 下载livox ros驱动程序
``` bash
git clone https://github.com/Livox-SDK/livox_ros_driver.git ws_livox/src
```

2. 构建livox ros驱动
``` bash
cd ws_livox
catkin_make
```

3. 更新ROS包环境
``` bash
source ./devel/setup.sh
```

> 到此，环境基本搭建完毕，可以使用如下命令启动Rviz连接激光雷达查看实时点云
> ```
> roslaunch livox_ros_driver livox_lidar_rviz.launch
> ```
> 
> https://github.com/Livox-SDK/livox_ros_driver/blob/master/README_CN.md

# 部分算法

## Livox_mapping

这个是Livox官方提供的建图软件包，适用于低速运动下的mapping。

![](<mid40_hall (1).gif>)

> 按上述环境搭建后，一般可以直接构建本软件包，如有提示缺少依赖，可以按官方文档安装PCL && Eigen && openCV
> openCV安装：https://blog.csdn.net/public669/article/details/99044895
> https://blog.csdn.net/qianbin3200896/article/details/107894029?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-10&spm=1001.2101.3001.4242
> 推荐openCV3.4的

#### 软件包构建

``` bash
cd ~/ws_livox/src
git clone https://github.com/Livox-SDK/livox_mapping.git
cd ..
catkin_make
source ~/ws_livox/devel/setup.bash
```

#### 运行

将雷达连接到电脑，依次执行如下命令（需要开两个终端）：
``` bash
roslaunch livox_mapping mapping_mid.launch
roslaunch livox_ros_driver livox_lidar.launch
```

随即将启动Rivz，移动激光雷达，完成建图。

> ##### 温馨提示
> 在Ubuntu 20.04系统下运行时，可能会出现一些问题，比如
> Error transforming odometry 'Odometry' from frame '/camera_init' to frame 'camera_init'
> 这时候需要将代码里的`/camera_init`改成`camera_init`
> https://www.cnblogs.com/xinzhaodc/p/16143348.html

## Livox_detection

Livox官方提供的激光点云目标检测算法，可以检测大部分车辆和行人。

<iframe src="//player.bilibili.com/player.html?isOutside=true&aid=896508475&bvid=BV1wA4y1D7qZ&cid=720277560&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

> github链接：https://github.com/Livox-SDK/livox_detection

#### 环境安装

``` bash
# python=3.8
# pytorch=1.8.2
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch-lts -c nvidia
# numpy=1.23.1
conda install numpy=1.23.1
# ros_numpy
sudo apt-get install ros-noetic-ros-numpy
# rospkg=1.4.0
pip install rospkg==1.4.0
```

#### 软件包构建

``` bash
git clone https://github.com/Livox-SDK/livox_detection.git
cd livox_detection
python3 setup.py develop
```

#### 运行

需要开四个终端

1. 运行roscore
``` bash
roscore
```

2. 运行模型
``` bash 
cd tools
python3 test_ros.py --pt ../pt/livox_model_1.pt
```

3. 点云发布
``` bash
roslaunch livox_ros_driver livox_lidar.launch
```

4. 可视化结果
``` bash
rviz -d ../tools/rviz.rviz
```