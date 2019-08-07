##  Hexo-CleanBlog
Hexo-CleanBlog是花了一天从这个主题[CleanBlog](http://www.codeblocq.com/assets/projects/hexo-theme-clean-blog/)修改而来，去除了浪费流量的图片，添加了标签、关于我等功能，以尽可能的保持简约。

## [hexo安装](https://hexo.io/)
```bash
# 安装npm
sudo apt-get install npm
# 安装hexo
npm install hexo-cli -g
# 安装rss工具
npm install hexo-generator-feed --save
# 初始化项目
hexo init blog
```


## GITHUB配置
1.在GitHub中建立一个仓库，比如说我的GitHub用户名是CodingCrush,我就要建立一个名为codingcrush.github.io的[repository](https://github.com/CodingCrush/CodingCrush.github.io)。
后台会自动在setting中自动开启GitHub Pages的功能，相当的简单。
![github_pages](/img/github_pages.jpg)
以后，更新博客主题与内容的操作都是通过GIT，仓库的操作很方便，如果不会用，可根据[廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)的教程进行学习。
Settings中还可以绑定到用户的独立域名上，设置好之后，仓库里会自动生成一个[CNAME](https://github.com/CodingCrush/CodingCrush.github.io/blob/master/CNAME)的文件，里面只有一行域名。

```bash
cd  blog/theme
git clone https://github.com/CodingCrush/Hexo-CleanBlog
```
另外，还需要增加About页面：
```bash
cd ..
hexo new page "about“
```
##  Hexo-CleanBlog配置
在blog目录下有一个_config.yml,这里会有几个比较关键的常量：
```yml
title: CodingCrush
description: Powered by Hexo and Hexo-CleanBlog
author: CodingCrush
# 时区与语言设置
language: en
timezone: Asia/Shanghai

# 切换为原版时间显示格式
date_format: MMM D, YYYY

# 更换主题
theme:Hexo-CleanBlog

# 部署: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/CodingCrush/CodingCrush.github.io
  branch: master

# RSS订阅: http://hexo.io/plugins/
plugin:
- hexo-generator-feed
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 10
```

在themes/Hexo-CleanBlog目录下，同样需要修改一个_config.yml文件
```yml
menu:
  Home: /
  Archives: /archives
  GitHub:
    url: https://github.com/CodingCrush/Hexo-CleanBlog
  # 增加About页面
  About: /about

comments:
  # Disqus comments
  disqus_shortname: codingcrush

# Google Analytics Tracking ID
https://www.google.com/analytics/注册后获取ID填上

mailto: codingcrush@163.com
# 增加rss订阅文件
rss: "/atom.xml
```
由于多说即将停止服务，还是继续用disqus吧，虽然不翻墙就看不了评论框

写博客时，使用markdown写作，生成的.md或者.markdown文件放在_posts目录下。
每篇文章首部要使用yml格式来写明这篇文章的简单介绍，格式如下：

## 使用markdown格式写作
新建博客：
```bash
hexo new post 'hello,world!'
```

```bash
---
layout:     post
title:      使用Hexo+Hexo-CleanBlog迁移Jekyll博客
tags:       [Hexo, GitHub,Jekyll, FrontEnd]
date:       2017-3-24 12:00:00
author:     "CodingCrush"
---
```

## Hexo常用命令：
```bash
hexo help #查看帮助
hexo init #初始化一个目录
hexo new "article" #新建文章
hexo new page "title" #新建页面
hexo generate #生成网页，可以在 public 目录查看整个网站的文件
hexo server #本地预览，'Ctrl+C'关闭
hexo deploy #部署.deploy目录
hexo clean #清除缓存，强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```
## Hexo部署
```bash
hexo generate
hexo deploy
```
