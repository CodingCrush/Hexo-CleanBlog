---
layout:     post
title:      说说菜鸟后端入职这半个月都干了些啥
tags:       [Career,Python,Linux,BackEnd,Pycharm]
date:       2016-11-19 9:00:00
author:     "CodingCrush"
---

# 前言
从10月30日到上海，到11月2日三天多，都在上海九号线沿途奔波找房，历尽各种困难，见识了各种坐地起价的黑心中介。从徐汇到七宝到九亭，2号晚上入职的前天晚上在泗泾找到了一间别墅的次卧，匆忙地让本科舍友从浦江镇开车把行李拖来。一间卧室一间书房，休息学习两不误，在寸土寸金的上海有这样的居住环境我已经非常满意。
![computer](/img/computer.jpg)
![bedroom](/img/bedroom.jpg)
11月3日正式入职，公司在漕河泾开发区，上海地铁九号线的拥堵让我叹为观止，早上都挤不上车，要知道我可是在泗泾啊！无法想象住在九亭七宝的朋友们挤地铁的艰难。每天单程55分钟在路上奔波，一天下来，和以前相比就少了2h的学习时间。回到住处，到睡前勉强能保证每天2h的学习时间。
# 小组介绍
公司算是IT界的传统行业，属于广电系统，我所属的部门主打教育。   
小组里的Y哥是复旦CS牛人，专业从事Python后端开发，名校帅哥，程序又写得好。   
J哥为人比较实诚搞笑，30岁才开始写C，之前写流处理，在常州有2套房，最近一心想跟着Y哥写Python。
前端有安卓、IOS，人员逐渐壮大，所以Y哥提议再招一个后端，我由Y哥面试后入职。

# Linux生产环境
公司要求使用Linux，我有多年的Ubuntu使用经验，最早使用的Ubuntu发行版是10.04，中间换过几次版本。然而公司都是用的CentOS，所以用起来还是一些不方便，我连YUM都不知道，竟然在CentOS终端下使用apt-get！！！！
第二天我便换成了Ubuntu16.10,因为16.04怎么都没装上，可能是刻盘过程中出现了一些问题。装个系统都能废掉一天时间，简直难以想象。过了两个星期，我还是在虚拟机中装了一个CentOS7,虽然都是LINUX，不同开发版日常使用区别不大，但项目部署时可能会影响进度，因此需要提早熟悉。

## 常用指令
scp,我连这个都不知道- -，第一次用还出错了，愚蠢：

    ssh root@remote ，连接到公共机上，
    scp 远程机文件 远程机所在目录

复制了几遍都是把远程机的文件复制到了另一个目录中

cat,less,more文本操作，  
grep 正则搜索  
find,文件搜索  

## 32位CentOS7 Server的安装
在虚拟机中安装CentOS7出现了一点小障碍，用VIRTUALBOX安装X86_X64混合版时，只能选择32位系统安装，然而一装遍黑屏，GOOGLE之后，发现CentOS即使是安装32位的，也必须要在X64环境下安装，我的主机是X64的，但无法选择64位安装方式。在BIOS里没有找到INTEL VT-X选项，看了看笔记本型号，发现笔记本年代久远，并不支持CPU虚拟化。  
更换VMWARE安装，可以选择识别为X64版本安装，但也出现黑屏问题，推断只能使用纯32位CentOS7安装。    
在[官方镜像](http://mirror.centos.org/altarch/7/isos/i386/)目录页下，下载300MB的Minimal版的再次安装，安装正常，但进入Terminal中ifconfig gedit 等指令都没有，300MB的精简版居然缺少这么多东西，这是我不曾预料到的。下载了4.3GB的DVD版再进行安装，选择了WEB SERVER版本，ADDONS全选，吃一堑长一智。

# Django与DjangoRestFrameWork
部门内使用Django框架进行开发，使用RESTFRAMEWORK框架提供RESTful API,最初的几天内，完成了django官网的相关教程，对django框架有了些简单的认识，由于有FLASK的经验，上手起来并不困难。

# 虚拟环境VENV
由于系统Python自带版本为2.7，为避免减少冲突，需要使用虚拟环境，Python3.5自带VENV，不需要使用第三方包VIRTUALVENV提供虚拟环境。

    python3 -m venv myvenv //创建虚拟环境
    cd myvenv 
    source ./bin/activate //激活虚拟环境
    deactivate            //关闭虚拟环境

用Pycharm自带的virtualvenv创建工具更加简单，方便。Pycharm找依赖包是从Python程序的目录为起点，树性向上搜索的？所以解释器所在的目录尤其重要。
![pycharm](/img/virtualvenv_pycharm.jpg)

# 代码检查工具flake8
项目中，Y哥要求使用flake8代码检查工具。  
项目代码要求通过flake8检查，通过检查后再提交到远程仓库。  
使用sudo安装flake8，再集成到PYCHARM中：  
![flake8](/img/flake8.jpg)  
实际上运行flake8检查，相当于在shell中调用flake8程序，传入的参数为当前文件的地址，$FilePath$即为文件目录地址的环境变量。   
除flake8之外，Python还可使用pylint进行静态检查。

# GIT

本来以为我掌握了GIT，但被Y哥纠正了几次，才逐渐明白GIT操作并非push pull fetch 这么简单，多人协作时，需要解决冲突问题，rebase stash等操作也必须掌握。尽量少创建branch，每一次对仓库的提交都必须是一次稳定可用的代码。
被打击了几次之后，我用PYCHARM内置的GIT了，试了几次之后，真心方便简单无脑。
JETBRAINS家的IDE真心好用，难以想象离开了它我写Python的效率会降低多少
![git](/img/pycharm_git.jpg)

# 其他
另外，接触到了swagger celery docker ipython等，需要深入了解。