---
layout: post
title: "octopress github blog"
date: 2017-01-19 16:38:51 +0800
comments: true
categories: 
---

[TOC]

# mac利用Octopress搭建一个Github博客

## 安装Octopress
在安装Octopress前，默认电脑中安装了ruby、git等环境。
### 下载Octopress
```
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
```
### 安装依赖
```
$ gem install bundler
$ bundle install
```
### 安装
```
$ rake install
```
## 配置Octopress
主要修改文件：_config.yml。

这个配置文件都有相应的注释。主要就是改一些博客头，作者名之类的东西。

注意最好把里面的 twitter 相关的信息全部删掉，否则由于 GFW 的原因，将会造成页面 load 很慢。修改定制文件 /source/_includes/custom/head.html 把 google 的自定义字体去掉，原因同上。

参考：[唐巧的博客](http://blog.devtang.com/2012/02/10/setup-blog-based-on-github/)
## 将博客部署到github上
### 建立github仓库
Github的[Page service](https://pages.github.com/)可以免费托管博客，并且还可以自定义域名。
建一个名为 username.github.io 的代码仓库。这里注意 username 必须是和你的账号名一致。访问地址为http://username.github.io
### 配置
利用octopress的一个配置rake任务来自动配置上面创建的仓库：可以让我们方便的部署GitHub page。在终端输入如下命令：
```
$ rake setup_github_pages
```

上面的命令会做一些事情(详细介绍看下面给出的参考链接)。其中最主要的就是创建一个_deploy目录，目录用来存放部署到master分支的内容。期间会要求你输入仓库的url，根据提示，进行输入即可。
参考：[利用Octopress搭建一个Github博客](http://justcoding.iteye.com/blog/1954645)
### 提交github
其中：
_deploy目录提交到master上，上述脚本已自动提交，博客原码提交到source分支下，需手动提交。


```
$ git add .
$ git commit -m 'Initial source commit'  
$ git push origin source  
```
## 写博客
写博客主要是用以下几个命令，[这里](http://octopress.org/docs/blogging/) 有详细介绍：

```
rake new_post[‘article name’] 生成博文框架，然后修改生成的文件即可
rake generate 生成静态文件
rake watch 检测文件变化，实时生成新内容
rake preview 在本机 4000 端口生成访问内容
rake deploy 发布文件
```
Octopress为我们提供了一些task来创建博文和页面。博文必须存储在source/_posts目录下，并且需要按照Jekyll的命名规范对文章进行命名：YYYY-MM-DD-post-title.markdown。文章的名字会被当做url的一部分，而其中的日期用于对博文的区分和排序。
 
通过Octopress提供的task可以正确的按照命名规范创建一个博文，并且在博文中会附带常用的一些yaml元数据。只需要在终端输入如下命令：

```
rake new_post["title"]  
```
其中title为博文的文件名，创建出来的文件默认是markdown格式。上面的命令会创建出这样一个文件：source/_posts/2013-08-03-title.markdown。打开这个文件，可以看到里面有如下一些内容了(告诉Jekyll博客引擎如何处理博文和页面)：
### 使用zsh执行命令注意地方
如果安装zsh，执行：
```
$ rake new_post["title.markdown"]
```
报错：
```
zsh: no matches found: new_post[title.markdown]
```

原因：zsh中若出现‘*’, ‘(’, ‘|’, ‘<’, ‘[’, or ‘?’符号，则将其识别为查找文件名的通配符

快速解决：用引号括起来```$ rake "new_post[arch-linux-reinstall-glibc.markdown]"```

彻底解决：取消zsh的通配(GLOB), 在.zshrc中加入alias rake="noglob rake"

> PS：在git回滚操作git reset --soft HEAD^时出现报错： zsh: no matches found HEAD\^，也为同样原因，因为\^也为正则中的符号， 需要转义为git reset --soft HEAD\^

### 发表博文的完整过程

```
$ rake new_post["New Post"]
$ rake generate
$ git add .
$ git commit -am "Some comment here." 
$ git push origin source
$ rake deploy
```
参考：[Blogging](http://octopress.org/docs/blogging/)

