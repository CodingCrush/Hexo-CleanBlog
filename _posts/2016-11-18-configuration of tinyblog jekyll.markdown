---
layout:     post
title:      配置TinyBlog-jekyll搭建GitHub静态博客
tags:       [Jekyll,GitHub,FrontEnd]
date:       2016-11-18 12:00:00
author:     "CodingCrush"
---

# Tiny Blog 
在学习Flask框架时，我用BootStrap模板里的[CleanBlog](https://blackrockdigital.github.io/startbootstrap-clean-blog/)为模板，MongoDB作数据库，搭建了一个简单的轻博客[Tiny Blog]( https://github.com/CodingCrush/TinyBlog)，在部署过程中，发现阿里云百度云主机成本实在过高`(简直是抢钱)`，对于个人来说并不是一个明智的选择。  
# Tiny Blog-Jekyll
逛知乎时，发现很多人推荐GitHub IO，诸多优点，用过的都说好。  
`你不能让我说好，我就马上也说好，第一我要试一下，因为我不愿意用完以后再加一些特技上去，博客“咣”一下，使用简单、界面很美，这样看官一定会骂我，根本没有这样的博客，就证明上面那个是假的。后来我也经过证实他们确实是对的，我搭了这个博客之后，感觉还不错，后来我在写这篇博客的时候也要写下来，因为我要让看官们知道，我用完之后是这个样子，你们用完之后也会是这个样子。`  
我便在之前的基础上，用了一个CleanBlog-Jekyll主题，去除了所有不必要的元素，尽可能的满足我对简约的追求。
## Jekyll的优点
使用过程中，越来越觉得这组CP真的很棒：
支持MarkDown语法，简洁方便，程序员居家必备；
网站结构简单，不需要过多操心；
托管在GITHUB服务器上，免费方便；
可以绑定独立域名，逼格不失；
GIT操作简单，只用两个指令`jkeyll build,git push`即可更新网站。
使用Jekyll可以节省时间，个人博客的根本意义还在于写作分享，把时间金钱都浪费在造轮子上岂不是舍本逐末？

## GITHUB配置
1.在GitHub中建立一个仓库，比如说我的GitHub用户名是CodingCrush,我就要建立一个名为codingcrush.github.io的[repository](https://github.com/CodingCrush/CodingCrush.github.io)。  
后台会自动在setting中自动开启GitHub Pages的功能，相当的简单。
![github_pages](/img/github_pages.jpg)
以后，更新博客主题与内容的操作都是通过GIT，仓库的操作很方便，如果不会用，可根据[廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)的教程进行学习。 
Settings中还可以绑定到用户的独立域名上，设置好之后，仓库里会自动生成一个[CNAME](https://github.com/CodingCrush/CodingCrush.github.io/blob/master/CNAME)的文件，里面只有一行域名。
## Jekyll主题
下载jekyll主题也比较简单，GITHUB上有大量的资源可供选择，如果使用我的TinyBlog-Jekyll,可以git clone 仓库。

       git clone https://github.com/CodingCrush/TinyBlog-jekyll
   
这是一个已经完整的Jekyll网站包，注意，只要推送`_site`文件夹内的文件到远程仓库。
## Jekyll环境搭建
Jekyll是用Ruby开发的，而Ruby的包是通过GEM管理的，因此便需要在本地先装好ruby与GEM，然后通过GEM指令进行安装。  
Linux下在Terminal中使用apt或yum安装ruby比较方便，windows可以在官方网站下载。
装完ruby之后就是要安装包管理器gem，自行搜索指令，我已经忘了我怎么装的了
最后安装jekyll，在Terminal里面输入：

     gem install jekyll 
     cd you website path //切换到你的网站目录下
     jekyll serve
     
如果不报错的话，那么便成功了。
我当时报错，另外安装了一个bundler后正常运行。
正常运行的网站运行在http://localhost:4000/

# TinyBlog-Jekyll配置
在根目录下有一个_config.yml,这里会有几个比较关键的常量： 

        # Site settings
        title: CodingCrush  //浏览器标签页名字
        email: codingcrush@163.com    //邮件地址
        copyright_name: TinyBlog-jekyll    //底部CopyRight 
        description: CodingCrush's blog    //描述
        baseurl: ""                        //网址前缀
        url: "http://codingcrush.me"       //或者为http://codingcrush.github.io
        github_username: CodingCrush       //GitHub帐号名
        zhihu_username :                   //填写后底部会生成知乎图标
        weibo_id: 2303345023               //填写后底部会生成微博图标
        disqus_shortname:                  //disqus评论系统的用户名
        duoshuo_shortname: codingcrush     //多说评论系统的用户名
        github_repository: TinyBlog-jekyll //填写后底部会生成项目仓库
 
最初我是用disqus的，但是不翻墙，无法显示评论框，相当不方便。多说虽然不及其稳定，也是可以用的。
写博客时，使用markdown写作，生成的.md或者.markdown文件放在_posts目录下。
每篇文章首部要使用yml格式来写明这篇文章的简单介绍，格式如下：

## 使用markdown格式写作
    ---
    layout:     post
    title:      配置TinyBlog-jekyll搭建GitHub静态博客
    tags:       [jekyll,github,前端]
    date:       2016-11-19 0:00:00
    author:     "CodingCrush"
    ---
    
layout是post，让jekyll知道你这是post排版的主题，
date必须按照yml的语法来写，日期格式必须正确，否则不显示，这里的时间是UTC时间，而填写timezone之后依然不能支持中国时间，比较疑惑。所以date的时间必须是当前时间减去8个小时，否则jekyll不予显示。

## 评论系统
GitPages是显示的静态网站，而评论系统是需要数据库支持的，因此想要在博客里使用评论系统，必须要有外部支持。一般用的评论系统有disqus和多说两种，到官网注册帐号，网站会提供动一个评论插件的代码，粘贴到post文件的网页模板里即可。这个主题里，在_config文件中填写用户名即可自动生成。  
`以下代码仅供参考，会被jekyll解释，以模板文件或官网代码为准`

    <div id="disqus_thread"></div>
    	<script type="text/javascript">
    	var disqus_shortname = '{{ site.disqus_shortname }}'; // required: replace example with your forum shortname
    	var disqus_identifier = '{{site.url}}/{{ page.url }}';
    
    	(function() {
    	var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
    	dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
    	(document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    	})();
    	</script>
    	<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    	<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
    </div>
                
    <div class="ds-thread" data-thread-key={{site.url}}{{ page.url }} data-title={{ page.title }} data-url={{site.url}}{{ page.url }}></div>
    <script type="text/javascript">
    	var duoshuoQuery = {short_name:'codingcrush';
    	(function() {
    	var ds = document.createElement('script');
    	ds.type = 'text/javascript';ds.async = true;
    	ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
    	ds.charset = 'UTF-8';
    	(document.getElementsByTagName('head')[0]
    	|| document.getElementsByTagName('body')[0]).appendChild(ds);
    	})();
    </script>
    
    