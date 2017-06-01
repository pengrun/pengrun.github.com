title: Mr.Peng解决Objective-C中Button点击延迟问题
date: 2015-08-03 13:40:46
categories: [Moblie]
tags:
- Objective-C
- Button
- 点击延迟

---
##简介
今天我一个同事突然问我一个问题，button高亮状态有延迟。他之所以发现此问题，是因为他在`rightBarButtonItem`中加入了一个button，在`scrollView`中加入一个button，两个button代码一直，但是在点击中会有不一样的效果，在`rightBarButtonItem`中的button点击能够立即处于高亮（按下状态），而在`scrollView`中的button为有大约`0.5s~1s`的延迟效果。

原文地址：[http://www.mrpeng.me/2015/08/03/Objective-C中Button点击延迟问题/](http://www.mrpeng.me/2015/08/03/Objective-C中Button点击延迟问题/)

<!--more-->

##排查问题

###切换buttonType
他发了一个demo给我，我不管切换`buttonType`为`UIButtonTypeCustom`还是`UIButtonTypeSystem`均不管用。

###scrollView换成self.view
把button加在`self.view`下面不会出现以上问题。

##锁定问题
以上两点可能锁定为是因为`scrollView`的问题，于是⌘+左键点击`scrollView`查看其属性，发现一个`delaysContentTouches`的属性。

```
@property(nonatomic) BOOL delaysContentTouches;       // default is YES. if NO, we immediately call -touchesShouldBegin:withEvent:inContentView:
```

##解决问题
默认是YES，如果为NO的话，会立即调用`-touchesShouldBegin:withEvent:inContentView:`方法。
然后回到demo中设置`scrollView`的`delaysContentTouches`为NO，运行。结果的确完美解决了上面的问题。
