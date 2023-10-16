---
title: OpenPCDet复现
tags: 复现记录, 目标检测, 点云, 深度学习, 激光雷达
date: 2023-10-16 20:00:00
comment: true
---

# 前言

# 环境搭建

https://www.cnblogs.com/mrneojeep/p/17390375.html

https://blog.csdn.net/qq_45228845/article/details/125583891

# 问题集合

1.  kornia问题
    ![](2023-10-16-19-58-11.png)
    ```
    ModuleNotFoundError: No module named ‘kornia‘
    ```
    或者
    ```
    Cannot statically infer the expected size of a list in this context
    ```

    > 解决方案：安装正确版本的kornia
    > https://blog.csdn.net/weixin_52288941/article/details/133518555

2.  demo跑通了但是：
    ```
    GLFW Error: X11: The DISPLAY environment variable is missing
    ```

    ssh远程没有Xserver客户端，显示不了可视化工具

    > 解决方案：配置本地可视化
    > https://blog.csdn.net/m0_50181189/article/details/120958568

