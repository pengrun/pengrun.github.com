title: 'Mr.Peng收集Hexo问题集'
date: 2016-08-05 11:24:02
categories: [转载]
tags:
- Mac
- Hexo
- 问题

---

1. markdown无法解析标题(#)
在`hexo3.0`前，在`##`后面直接写标题然后`deploy`都能识别标题，但是在`hexo3.0`后`##`直接加标题就无法解析了。
	* 解决办法：在`##`和后面字符之间加一个空格,如果不加空格 有些引擎就解析不了，如：
	```
	### hello world
	```
参考[The"###"markdown singal does not work with release 1.3.0 #81 Daring Fireball: Markdown Syntax Documentation](The"###"markdown singal does not work with release 1.3.0 #81 Daring Fireball: Markdown Syntax Documentation)
	
	
	