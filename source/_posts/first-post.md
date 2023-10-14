---
title: 建站小记
tags: 技术总结
date: 2023-10-13 10:00:00
---
记录本博客搭建过程。

为了以后更好的记录学习过程、问题解决以及一些随笔，特建立个人博客，开始养成写博客的习惯。

# 安装Hexo

## 环境

* Node.js

* Git

> 安装过程不再细讲，主要吐槽一下用宝塔面板安装node.js时各种问题：npm版本过高、命令行命令无效等。\
> 所以没有必要在服务器上装，本地电脑安装后，后面有部署到GitHub Page的步骤。

## 安装

环境安装完，直接用npm安装Hexo

``` bash
$ npm install -g hexo-cli
$ hexo init ***.github.io  # 这里替换成你自己的，为后续更新到github上，使用github名字.github.io
$ cd ***.github.io  # 进入本地的博客文件夹
$ npm install
$ hexo server # 打开本地服务器预览
```

接着安装fluid主题，官网：[Fluid](https://hexo.io/themes/)

``` bash
$ npm install --save hexo-theme-fluid
```

或者选择直接去GitHub下载[release源码](https://github.com/fluid-dev/hexo-theme-fluid/releases/tag/v1.9.5)，解压到hexo项目的themes目录下，重命名为fluid

## 配置

修改博客目录中_config.yml

```
language: zh-CN  # 指定中文
theme: fluid  # 指定主题
```

更多配置可以参考Hexo官方文档：[Hexo 配置](https://hexo.io/zh-cn/docs/configuration)，以及Fluid官方文档：[Fluid 配置指南](https://hexo.fluid-dev.com/docs/guide/)。Hexo配置是博客主目录下的_config.yml，Fluid配置是themes/fluid目录下的_config.yml

## 本地部署

执行如下命令，可以在本地生成静态页面

``` bash
$ hexo clean  # 清空一下缓存，有时候博客页面显示不正常也可以试试这个命令行
$ hexo g  # hexo generate的简写，把刚刚做的改动生成更新一下
$ hexo server  # 在本地服务器看看博客：https://localhost:4000
```

到此你已经可以在本地看到你的博客小站啦~

# 部署Github Page

Github Page是提供静态网站访问服务的，就是将静态网站项目传到GitHub上，他能为你提供一个url入口（如rlin1538.github.io），直接访问你的静态网站，而Hexo编译生成的就是一个静态网站。

首先我们得把生成的静态网站上传到GitHub中，GitHub中得有一个对应的仓库。所以第一步是创建一个名为`GitHub用户名.github.io`的代码仓库![](001.png)