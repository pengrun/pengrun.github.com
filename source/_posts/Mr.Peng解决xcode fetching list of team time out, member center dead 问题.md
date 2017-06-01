title: 'Mr.Peng解决xcode fetching list of team time out, member center dead 问题'
categories: [Pc]
tags:
- Mac
- Xcode

---

## 简介
今天一个朋友遇到以下情况，如下图：

<!--more-->

![image](http://ww3.sinaimg.cn/large/006tKfTcgw1f6vm931a8yj30hh0a5js1.jpg)
等了很久最终出现以下俩结果：
![image](http://ww4.sinaimg.cn/large/006tKfTcgw1f6vm92vwj0j30gv0ai74s.jpg)
![image](http://ww3.sinaimg.cn/large/006tKfTcgw1f6vm92d675j30es07d74s.jpg)
## 解决
一看，显然是本机的`DNS`出现问题了。于是有以下俩方法能够解决此问题：

1. 开启`VPN`可以解决此问题;
2. 查看本机DNS是否加入`114.114.114.144`,如果没有加入最好加入上，这样就不用开`VPN`了。

