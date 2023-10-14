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

``` yml
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

然后修改博客配置_config.yml，改成你的信息

``` yml
deploy:
  type: git
  repo: git@github.com:xxxx/xxxx.github.io.git # xxxx是你的GitHub用户名
  branch: master
```

然后生成站点文件并推送到远程仓库即可，推送时需要填GitHub账户和密码

``` bash
hexo clean  # 保险起见，每次我们都清空一下
hexo deploy --generate
```

推送完成后，打开你的Github Page即可看到小站啦！（可能会有一分钟左右的延迟）

参考：[部署|Hexo](https://hexo.io/zh-cn/docs/one-command-deployment)

# 创作

要开始写博客，可以通过在博客目录执行如下命令创建一篇新文章：

``` bash
hexo new test
```
hexo会在source/_posts目录下创建一个test.md文件，我们在这里使用markdown语法写博客即可

或者你可以直接在source/_posts目录创建md文件。

``` yml
---
title: test
date: 2022-04-22 21:17:39
tags: hexo
categories: 教程
---

# 一级标题

## 二级标题

这里开始我们的写作吧
```

上面文件最上方以---分隔的区域，用于指定文章属性（[Front-matter | Hexo](https://hexo.io/zh-cn/docs/front-matter)），常用的属性就是title, date, tag, categories，分别指定文章标题、建立日期、标签和分类。其中标题和建立日期都是自动生成的，我们不用管。tag和categories一般是由我们自己设置的

# 自定义域名

我们可以自己买个域名，然后把GitHub Page绑定到自己的域名下。
我用的域名商是[NameSilo](https://www.namesilo.com/)，十几块就能买到一个一年的域名，你也可以选择其他，比如国内的[阿里万网](https://wanwang.aliyun.com)

首先需要在博客项目source目录中创建一个CNAME文件，填入你的域名：

```
nuaa.life
```

接着我们进入域名服务商控制台，配置域名解析：![](2023-10-14-13-27-21.png)
目标填你的GitHub Page地址，然后在GitHub博客项目中点Settings，选择Pages，填写Custom domain为你的域名，然后你就可以通过你的个人域名访问博客小站啦

> 国内的域名商一般自带的控制台服务挺好用，DNS配置方便
> 国外的可以用[Cloudflare](https://cloudflare.com)来配置域名解析，如何使用网上已有很多教程。

参考文章：https://juejin.cn/post/7090201115005812767

# 自动部署

到此为止，我们需要在本地去编译网站静态文件，然后上传。

为方便博客撰写以及源码备份，我们可以采用Github Action自动部署，即将本地hexo源码上传到GitHub（不是上文提到的编译后的静态网站文件），利用GitHub提供的Actions，自动流转流水线编译出静态网站文件传到rlin1538.github.io仓库中。

这样，我们只需在本地写md，然后用git提交push到GitHub中即可，十分的方便！

本站参考了这篇文章完成自动部署，友友可以参考：[利用Github Actions自动部署Hexo博客](https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/)


---

> # 开始享受你的博客之旅吧~~~