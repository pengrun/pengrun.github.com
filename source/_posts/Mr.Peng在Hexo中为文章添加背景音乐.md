title: 'Mr.Peng在Hexo中为文章添加背景音乐'
date: 2015-06-25 17:28:13
categories: [Web]
tags:
- Hexo
- 折腾

---
## 添加背景音乐
### 豆瓣电台
> 个人推荐豆瓣电台，所以这篇文章只讲如何添加豆瓣电台到文章中。如果想添加其他的平台的到文章中，请移步度娘或者谷歌。
> 推荐原因其实很简单，在决定写这篇博客前，我肯定是扒了很多大神的文章，有添加虾米、新浪、QQ的，但是觉得**豆瓣电台**符合我的口。
> 优点：自动播放、随机播放
> 缺点：不能指定播放
<!--more-->
## 增添方法
复制下面代码到你的博文任意位置，然后`Hexo s`预览。建议放在最下面，如果你放在最上面，很有可能别人还未点击进入博文，你的博客就有了背景音乐了。
	
	```
	<center> <iframe name="iframe_canvas" src="http://douban.fm/partner/baidu/doubanradio" scrolling="no" frameborder="0" width="400" height="200"></iframe> </center>
	```
<center> <iframe name="iframe_canvas" src="http://douban.fm/partner/baidu/doubanradio" scrolling="no" frameborder="0" width="400" height="200"></iframe> </center>
