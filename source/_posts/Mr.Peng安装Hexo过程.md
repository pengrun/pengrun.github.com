title: Mr.Peng使用Mac安装Hexo搭建博客
date: 2015-06-18 16:14:53
tags:
- Hexo
categories: [Web]

---
由于自己只学习过OBC，所以对其他语言有所欠缺，最终决定自己搭建一个Blog来强迫自己学习下另外个语言，也顺便记录下自己在iOS开发过程中的体会和经验。
##Mac安装Hexo的步骤
根据[官方文档](https://hexo.io/docs/)的步骤，我来依葫芦画瓢的给大家参考下我自己的安装过程。
###安装环境：
Mac OS X Yosemite 10.10.3

本文的编辑器：[Mou](http://25.io/mou/)
###安装需求：
Node.js

Git
<!--more-->
###Git
建议去下载[GitHub for Mac](https://mac.github.com/)

因为图形化的Github登陆之后会自动在本地配置好SSH，执行git shell时也不需要-config username和Email

新建的仓库，github pages 的仓库名必须为your_user_name.github.io，比如我的your_user_name为pengrun，那么我的仓库名必须为pengrun.github.io，如果不使用your_user_name来作为仓库名的话，那么后面deploy后进入pengrun.github.io会出现404。

如果不使用图形化界面的话，就需要配置SSH，参考[SSH配置教程](https://help.github.com/articles/generating-ssh-keys)

###Node.js
我的mac是安装了xcode,而且mac自带Git，所以我直接用git clone来安装的，打开终端，依次输入下面的命令

	$ git clone git://github.com/joyent/node.git
	$ cd node
	$ ./configure
	$ make
	$ sudo make instal
如果不喜欢用终端来安装的话，其实可以直接去[Node.js](https://nodejs.org/download/)官网下载。

###安装Hexo
安装好了Node.js后就可以用npm命令来安装Hexo

	$ npm install -g hexo-cli
创建项目folder自己命名

	$ hexo init<folder> 
进入目录

	$ cd <folder>
初始化项目
	
	$ hexo init
	
安装依赖包

	$ npm install
后面所有的命令均在folder里面进行

启动服务

	$ hexo s
	
如果终端显示

	$ Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
用浏览器打开`http://localhost:4000/`或者`http://127.0.0.1:4000/`就能看到网页了,`Control + C`可以结束于预览

###写一篇博客
新建文章
	
	$ hexo new "第一篇博客"
在_posts目录就会生成文件```第一篇博客.md```

	title: 标题
	date: 2014-11-11 11:11:11
	tags:
	- 标签1
	- 标签2
	- 标签3
	
编辑完成之后保存，执行```hexo s```进行预览
###主题更换
主题更换可以去看下hexo在github里面梳理的目前很多流行的主题，具体如何更换我就不做介绍了。[主题地址](https://github.com/hexojs/hexo/wiki/Themes)
###发布
编写在前面创建的```folder```文件中的```_config.yml```中```deploy```部分，以```pengrun```为例

	deploy:
  		type: git
  		repo: https://github.com/pengrun/pengrun.github.io.git
  		branch: master
`type` 这里的必须为```git```，很多教程里面写的是```github```，如果是```github```的话，在你后面执行```hexo d```时最后会出现一个下面这个错误

	$ ERROR Deployer not found: github
	
`repo` 这里最好使用`git@github.com:your_user_name/youer_user_name.github.io.git`格式。
###部署
	$ hexo d
如果成功会得到以下提示：

	[info] Deploy done: github
不出问题的话，应该几分钟之内打开。如果按照步骤一步一步来的话几分钟内无法打开，请等待10分钟左右再试试