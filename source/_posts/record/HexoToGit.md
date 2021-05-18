---
title: hexo+Github 在windos上搭建个人网站
date: 2020-04-20 16:53:13
description: 简简单单的记录一下我的建站过程
tags: 
  - Hexo
  - 教程
categories: 
  - 雨筝的记录鸭
---

## 前言

一开始本来打算建一个动态网站的(顺便屯点东西~~仓鼠精本精~~),但由于资金不足，懒于维护就此作罢。也在本地搭建一个小网站，做了内网穿透，但太不稳定，再次作罢。然后用Hugo做了一个静态网站并部署到了GitHub上，但网上教程太少，主题也少，没两天便换成了现在用Hexo，这应该是最终的结果了。
这里呢，只是一个简单的记录。
如果你想要更详细的Hexo建站指南，推荐[云游君](https://www.yunyoujun.cn/)的[教你如何从零开始搭建一个属于自己的网站](https://www.yunyoujun.cn/share/how-to-build-your-site/)
如果你想要视频的话推荐[这里](https://www.bilibili.com/video/BV1Yb411a7ty)


<!-- more -->

## 步骤

### 安装Git

选择对应版本下载安装即可   
[git下载](https://git-scm.com/) （如果觉得国内速度太慢，可以试试[这里](https://pc.qq.com/detail/13/detail_22693.html)）

### 安装Node.js 

选择对应版本下载安装即可   
[Node.js下载](https://nodejs.org/en/download/)

### 安装hexo

前面git和nodejs安装好后，就可以安装hexo了。  
你可以先创建一个文件夹，然后cd到这个文件夹下（或者在这个文件夹下直接右键git bash打开)

输入命令
```sh
# 全局安装hexo
npm install -g hexo-cli
```
输入`hexo -v`和`npm -v`查看是否安装成功


接下来初始化`hexo`
```sh
# 初始化博客的模版文件 后面跟的myblog是你要新建的文件夹
hexo init myblog 
#进入创建的myblog文件夹
cd myblog
# 默认安装所有 `package.json` 文件中提到的包
npm install
```
开启本地的 Hexo 服务器 
```sh
# 也可以缩写为 hexo s
hexo server 
```
现在在浏览器输入localhost:4000就可以看到你生成的博客了。

按 Ctrl + C 结束服务器的运行

### 修改主题

找到一个喜欢的主题 比如我现在使用的[hexo-theme-yun](https://github.com/YunYouJun/hexo-theme-yun)主题，作者：[云游君](https://www.yunyoujun.cn)

把主题下载到themes主题文件夹
```
git clone -b master https://github.com/YunYouJun/hexo-theme-yun themes/yun
```

下载依赖
```
npm install hexo-render-pug hexo-renderer-stylus
```
修改根目录下的`_config.yml`
找到
```yml
theme: landscape
```
修改为
```yml
theme: yun
```
至此主题就修改成功了
```
hexo clean
hexo g
hexo s
```
然后你就可以在本地localhost:4000看到新修改的主题了  
[更多关于此主题的配置](https://yun.yunyoujun.cn/)

#### 关于`_config.yml`

```yml
title: 网站标题
subtitle: 副标题
description: 描述
author: 作者
#语言
language: zh-CN
#网站地址
url: http://raincither.com
```


### 新建文章

创建一篇新文章或者新的页面
```
hexo new [layout] <title>
```
> layou 为你创建文章的布局 默认为 post          
> title 为你要创建文章的名称，你也可以指定路径 默认路径为source/_posts

示例：
```sh
# hexo new 可以简写为 hexo n
# 在source/_posts下创建名为first的markdown文件
hexo n first
#在source/first下创建名为index的markdown文件
hexo n page first
```


### GitHub创建个人仓库

首先，你要有一个[GitHub](https://github.com/)账户，没有的化去[注册](https://github.com/join?source=header-home)一个。

注册完登录后，点击右上角的 + -> New repository 新建仓库。   

创建一个仓库名为：`你的用户名.github.io`，用户名大小写无所谓，但建议小写。  
  
要使用GitHub page服务部署必须以这种格式创建仓库，才会被识别，也就是`xxxx.github.io`。 

点击`create repository`

### 将hexo部署到GitHub

安装hexo-deployer-git 插件
```sh
npm install hexo-deployer-git
```
然后打开 _config.yml 拉到最后找到下面的内容
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:    
    type:
```
修改为
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git  
  repo: https://github.com/你的用户名/你的用户名.github.io.git
  branch: master
```

然后执行
```sh
# 为了避免受错误缓存影响，最好使用 hexo clean 先清除一遍 此时会删除public文件夹
hexo clean
# 重新生成静态文件即public文件夹 可以简写为 hexo g
hexo generate
# 推送到GitHub仓库中，可以缩写为 hexo d
hexo deploy
```
> deploy时可能要你输入username和password。

等待完成后，打开网址 `https://你的名字.github.io` 就能看到你的网站了

### 更新
 
本地更新
```sh
hexo clean
hexo g
```
本地预览调试
> 这一步可以跳过
```sh
hexo s
```
更新到GitHub
```sh
hexo d
```
## FAQ

### npm 过慢问题
一开始我的解决方法是安装 `cnpm` 来解决的，但是版本太低,致使后面出现了一些问题，这里附上新的解决方案。
```
npm config set registry https://registry.npm.taobao.org

npm install -g nrm

nrm use taobao
```

### hexo 在 Windows上 无法执行 
报错：
因为在此系统上禁止运行脚本

解决方法：

以管理员运行Powershell输入以下命令

```sh
# 默认执行策略为Restricted   不允许任何script运行
# 更改执行策略为RemoteSigned 运行本地的script不需要数字签名
Set-ExecutionPolicy RemoteSigned
```
然后输入 `a` 回车 

现在就可以使用 `hexo` 了
