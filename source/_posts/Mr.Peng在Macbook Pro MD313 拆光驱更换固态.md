## Mr.Peng在Macbook Pro MD313 拆光驱更换固态

title: 'Mr.Peng在Macbook Pro MD313 拆光驱更换固态'
categories: [Pc]
tags:

- Mac
- MD313
- 折腾

---

### 前言

小猿的Macbook是2011年末的，大3时候买的，为的就是学习iOS开发。500GB的硬盘，4GB的内存，打开各种软件挺流畅的，并且还装了双系统，玩剑网三和LOL。自从升级到了`OS X 10.9 Mavericks`之后，就开始迟钝了，于是14年就升级了内存，买了根`金士顿ddr3l 1600 8g笔记本内存条8g ddr3 低电压12800S兼容4G`[前往购买地址](http://s.click.taobao.com/xw7yNPx )升级到了10GB内存，之前网上说会掉频，但是经实战为掉频，并且用了快两年了。

<!--more-->

![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9cjp2v9xlj31kw16o7qs.jpg)

![](http://ww3.sinaimg.cn/large/006y8mN6gw1f9cjop9w6fj31kw16oh55.jpg)

但是到了16年10月升级`siessa`之后，各种风火轮、菊花残。开机从“duang”一声到选择账户界面读条可能要花50几秒，然后选择了账户输入了密码之后，lanuchPad加载完毕可能又要接近2分钟，也就是说开机到正常使用要花3分钟，然后就是打开xcode、word、ps等软件，对应的图标可能要跳跃30几下。最终于16年10月26日，爬了很多文，决定把光驱盘拆了，换上SSD。

### 警告

拆机有风险，换系统也有风险。 最好先用`TimeMachine`进行备份，不然所有责任自负

### 准备

1. SSD

   对于SSD，准备3个品牌

   * Intel 730 240G[前往购买链接](http://s.click.taobao.com/WnxlOPx)
   * ![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9cl006uh9j30rc0c1q5o.jpg)
   * 浦科特 m6s 256 G[前往购买链接](http://s.click.taobao.com/IiujOPx)![](http://ww3.sinaimg.cn/large/006y8mN6gw1f9cl0kl4g7j30qs0b0gna.jpg)
   * 三星 850 EVO 250 G [前往购买链接](http://s.click.taobao.com/kcRiOPx)![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9cl153llcj30r10cg40k.jpg)

   通过爬文，最终选择`Intel 730 240G`

2. 硬盘托架

   由于把光驱位换成SSD，必须得购置一块硬盘托架地址[前往购买地址](http://s.click.taobao.com/rZRdOPx)![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9cl1vi2epj30q20bfgon.jpg)

3. USB光驱

   如果需要使用光驱的话，可以把拆下来的光驱放到USB光驱里面，就可以作为外置光驱盘[前往购买地址](http://s.click.taobao.com/sQwcOPx)

### 备份

在购买了所有东西之后，在等待货到前，通过`TimeMachine`备份一下数据，一些不重要的东西就不必备份了，比如下载的各种片啊、音乐、图片等占内存的。



### 组装

等购置的东西到了之后，就可以参照托架上的步骤把SSD插上。![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9chxcx0z4j31kw23ukjl.jpg)

插上之后，别忘了把托架两边的螺丝上紧。![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9ch8ljoosj31kw16o1kx.jpg)

![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9ch8gubqhj31kw16o1kx.jpg)

### 拆机

1. 拆后盖

   把后盖上面的所有螺丝全部拧下来，新手的话最好按照拆的方位放好，如下图：![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9cmvsuk0pj31kw23uu0x.jpg)

2. 拆光驱

   * 把光驱周围的9个螺丝全部拧下来，由于很激动，忘记照相了，不过我记录了位置（下图右下角那个螺丝很隐秘，请大家注意看），大家可以参考下，如下图：![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9chu1jam8j31kw16oqmo.jpg)


* 螺丝拆完之后，把光驱旁边的三组线对应的接头扣出来，如下图![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9cn0cgt7dj30sg0l8dle.jpg)
* 接着就可以慢慢取下光驱了
   * 在取下的光驱中，有个一个伸出的小脚，然后把小脚轻轻拔出来，如下图![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9cn33sf47j30l80sgdl2.jpg)
   * 把小脚插进硬盘托架上
   * 最后把拆下的光驱装进USB光驱盒里面。

### 装SSD

1. 把装好小脚、插好SSD的硬盘托架，慢慢插入到原光驱位；
2. 之前光驱位周边的三组线接口重新对应的插上；
3. 如果很自信的话，就把螺丝拧上，如果不自信，先别拧螺丝，小猿我就是一个不太自信的，就没有拧。盖上后台，开机启动，点击`左上角`的`关于本机`中的`系统报告`找到`SATA/SATA Express`看是否能够找到SSD。如下图![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9cnb3pq2aj30kv0egjtu.jpg)

### 恢复系统

1. 重启，当听到“duang”的一声，一直按住`command+R`直到看到苹果标志之后，松开键盘，等待读条（如果不进入恢复模式，直接按`option`进入选择系统界面，是不会出现ssd的，因为ssd里面没有引导文件）；
2. 进入恢复界面之后，点击左上角苹果，打开`磁盘工具`，抹掉（格式化）新加入的SSD，犹如我健忘，就以HDD为例，格式选择`Mac OS 拓展（日志式）`，方案选择`GUID分区图`如下图![](http://ww3.sinaimg.cn/large/006y8mN6gw1f9cipwbpgwj30s90cl0up.jpg)
3. 退出`磁盘管理`，然后选择`TimeMachine`进行恢复系统![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9ciywff5pj30ku0fm76h.jpg)
4. 等待读条，完毕之后就按照步骤输入iCloudID等进行激活。

### 上螺丝

激活之后，关机，把螺丝上好。！

### 优化SSD

1. 打开Trim（必须）

   前往三方工具官网下载最新的`Trim Enabler`[前往下载地址](https://www.cindori.org/software/trimenabler/)打开Trim，如下图

   ![](http://ww3.sinaimg.cn/large/006y8mN6gw1f9cj63r2luj30910deaan.jpg)


2. 关闭TimeMachine本地备份功能（建议）

   因为SSD的内存本来就不是很大，就没必要本地TimeMachine备份了，如果需要可以外接一个移动硬盘作为备份，在终端输入：

   ```objective-c
   $ sudo tmutil disablelocal  
   ```

3. 关闭Spotlight（看个人喜好）

   `Spotlight`是OS X自带的文件索引工具，对于小猿我喜欢用`Alfred`的来说，这个`Spotlight`就是一个累赘，所以把他关闭了，当然这个看个人喜好，在终端输入：

   ``` 
   $ sudo mdutil -a -i off
   ```

   ​

4. 关闭Dashboard（建议）

   在 Dock 或者launchPad点按 Dashboard 图标，或者4个指母轻滑触控板也能打开Dashboard，小猿我其实很少使用这个，所以也把它毙了，在终端输入:

   ```
   $ defaults write com.apple.dashboard mcx-disabled -boolean YES
   $ killall Dock
   ```

   如果想打开Dashboard，在终端输入

   ```
   $ defaults writecom.apple.dashboard mcx-disabled -boolean NO
   ```

5. 关闭深度睡眠(不建议)

   当Mac没电时候，系统会自动进入深度休眠，把把内存存到硬盘上相同大小的空间中,很少用到也占用硬盘空间。通过下面命令查询：

   ```
   $ sudo pmset -g | grep hibernatemode
   ```

   然后会有如下相应：

   ```
   hibernatemode	3
   ```

   如果是3表示是有深度休眠的。

   想关闭的话，在终端输入：

   ``` 
   $ sudo pmset -a hibernatemode 0
   ```

   想删除深度休眠创建的内存映像文件，在终端输入：

   ```
   $ sudo rm /var/vm/sleepimage
   ```

### 效果

上面教程已经把SSD装好了。现在来看下我的测试

1. 从开机“duang”到launchPad加载完毕，只花了10几秒；
2. 打开Xcode，Xcode只跳了3下；
3. 打开OmniGraffle，OmniGraffle只跳了4下；
4. 打开word，word只跳了5下；
5. 用Disk Sensei 测试一下速度，如下图：![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9cq9295tyj30eg0iwjt9.jpg)

### 总结

三个字总结：快、狠、准；



PS：附上一张被偷拍的图片

![](http://ww3.sinaimg.cn/large/006y8mN6gw1f9ey2tq1q1j311i1e0qpd.jpg)