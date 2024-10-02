---
title: CFM键位码第三方小工具
tags: 
    - android
    - kotlin
date: 2024-03-15 10:00:00
comment: true
---

# CFM键位码工具

## 简介

本软件是穿越火线：枪战王者的第三方键位码工具，支持上传本地键位、使用键位码替换本地键位、键位查看功能。

### 下载

### [CFM键位码工具V1.1.apk](/download/CFM_Tool.apk)

### 开源地址
https://github.com/rlin1538/Cfm_Joy_Manager

### 应用截图
<center class="half">
<img src="https://github.com/rlin1538/Cfm_Joy_Manager/assets/60032065/858f00c3-000e-4ae5-a454-3bffc8cea657" width=200/>
<img src="https://github.com/rlin1538/Cfm_Joy_Manager/assets/60032065/109306f4-f43d-43dd-be30-8ce9c623fb68" width=200/>
<img src="https://github.com/rlin1538/Cfm_Joy_Manager/assets/60032065/39fd6c72-5a58-4250-82f6-e28fcd7ff7cb" width=200/>
</center>

![Screenshot_2024-03-18-13-17-12-158_com rlin cfm_joy_manager](https://github.com/rlin1538/Cfm_Joy_Manager/assets/60032065/3d816563-4d39-4fc8-ad35-34b9d459a360)



## CFM键位码第三方小工具开发记录


### 需求分析

CF手游的键位码系统官方迟迟做不出来，因此心血来潮，自己搞一个出来，如果能传播开，用的人多了，还能敦促官方尽快搞出来第一方的。

首先分析整个系统的基本需求
- 键位修改
- 键位查看
- 云存档
- 键位码系统

玩家原本可以用MT管理器，手动进入键位文件`CustomJoySticksConfig_*.json`修改具体键位数据，从别的地方复制过来粘贴，有许多代练代上分的玩家登录别人的好需要自己手动替换一遍，比较麻烦。而隔壁游戏和平精英，早早的就推出了官方键位码系统，和平营地还能去找热门主播的键位码来用（不愧是手游FPS头把交椅，策划比CFM优秀太多）

本工具旨在完成一个第三方的键位码系统，方便玩家更换键位。

基础场景为：用户选择本地的一个键位文件（对应的一个用户的一个键位方案），点击修改，输入键位码后，即可完成键位的一键替换。键位文件将存放在云端，需要有一个数据库存储，用户可以将本地数据上传到云端，并返回一个键位码，唯一标识其上传的这个数据。

不设置用户系统，暂无必要


### 系统设计

采用Android原生客户端+Supabase为后端的系统设计

Android采用原生安卓的Jetpack Compose库来搭建

Supabase是类似Firebase的Baas（backend as a service）服务商，对app开发者很方便，无需自己搭建后端系统。

### 技术分析

#### 键位查看
分析键位文件的结构，定位用xPos和yPos来完成，其他还有透明度、大小等属性，但是不同按键的坐标原点不一样（策划这么搞一定有他的道理），共八个坐标系：左上、左中、左下、中心点、中下、右上、右中、右下。

需要按键都分类出来，然后对应到坐标系中。比较简单的方法是将键位json中的所有xPos和yPos改成0，然后能比较快速的分类。

然后剩下工作就比较简单了，用Canvas直接画就行。

#### Android/data/* 权限

在Android11之后，谷歌加入了文件沙箱存储模式，第三方应用就不能像以前一样直接访问Android/data目录了。

新的方法是用SAF框架来授予应用某个文件夹的权限。

> 本文依据这篇博客的方案来解决权限问题：https://cloud.tencent.com/developer/article/2106318

#### 后端服务

自己从头搭后端太麻烦了，直接用 [Supabase](https://supabase.com/)，注册后建个项目，然后建个表就直接能用了。
客户端安装对应的SDK，直接调用，特别方便（关键还是免费的

> Kotlin的使用指南：https://supabase.com/docs/reference/kotlin/gt

### 客户端
![home](Screenshot_2024-03-15-17-35-51-262_com.rlin.cfm_j.jpg)
![native](Screenshot_2024-03-15-17-35-55-496_com.rlin.cfm_j.jpg)
![cloud](Screenshot_2024-03-15-17-35-58-254_com.rlin.cfm_j.jpg)