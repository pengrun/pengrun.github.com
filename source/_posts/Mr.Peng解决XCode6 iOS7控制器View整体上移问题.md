title: Mr.Peng解决XCode6 iOS7控制器View整体上移问题
date: 2015-06-30 10:22:55
categories: [Moblie]
tags:
- Objective-C
- Original

---
在项目中，在适配iOS7时，遇到了下面的问题，请看下图：
![iOS Simulator](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/01iOS%20Simulator%20Screen%20Shot%202015%E5%B9%B47%E6%9C%881%E6%97%A5%20%E4%B8%8A%E5%8D%889.24.22.png)
<!--more-->
控制器的内容整体上移了大概44像素。而44像素对iOS开发人员来说，肯定比较敏感。而且能够发现`navigationBar`是透明的，猜测：可能是导航栏透明的问题。动手试一试。在controller中的`-(void）viewDidLoad`中加入
	
	```
	self.navigationController.navigationBar.translucent = NO;
	```
现在`⌘+R`运行下。结果如下图：
![iOS Simulator](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/01iOS%20Simulator%20Screen%20Shot%202015%E5%B9%B47%E6%9C%881%E6%97%A5%20%E4%B8%8A%E5%8D%889.24.33.png)
综上所述：猜想正确。

现在Google一下为什么呢？
> PS:肯定很多人在想为什么我能Google呢?推荐一个免费+收费翻墙软件,支持iPhone、Android、Mac、Windows。地址：[http://mayi.08dream.com/share/285f89b802bcb2651801455c86d78f2a](http://mayi.08dream.com/share/285f89b802bcb2651801455c86d78f2a)，里面有相应设备的配置。

回归正题。在StockOverflow上面也有人在问相同问题。传送门:
[https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW1](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW1)
这个人自问自答了，而自答的答案也是设置navigationBar的translucent = NO。最下面有一个更好的解决方案，就是在`-(void）viewDidLoad`中加入

	```
	if ([self respondsToSelector:@selector(edgesForExtendedLayout)])
        self.edgesForExtendedLayout = UIRectEdgeNone;
	```
来解决此问题。我试了两种方法之后，感觉都是达到同样的目的，但是我还是觉得第二种为最佳解决方法，之所以我认为是最好的解决，是因为`[apple doc](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW1),明确说明了此情况。

> iOS 7 brings several changes to how you layout and customize the appearance of your UI. The changes in view-controller layout, tint color, and font affect all the UIKit objects in your app. In addition, enhancements to gesture recognizer APIs give you finer grained control over gesture interactions.

> **Using View Controllers**

> In iOS 7, view controllers use full-screen layout. At the same time, iOS 7 gives you more granular control over the way a view controller lays out its views. In particular, the concept of full-screen layout has been refined to let a view controller specify the layout of each edge of its view.

> The `wantsFullScreenLayout` view controller property is deprecated in iOS 7. If you currently specify `wantsFullScreenLayout = NO`, the view controller may display its content at an unexpected screen location when it runs in iOS 7.

> To adjust how a view controller lays out its views, `UIViewController` provides the following properties:

> * **edgesForExtendedLayout**
> 
> The `edgesForExtendedLayout` property uses the `UIRectEdge` type, which specifies each of a rectangle’s four edges, in addition to specifying none and all. Use `edgesForExtendedLayout` to specify which edges of a view should be extended, regardless of bar translucency. By default, the value of this property is `UIRectEdgeAll`.

> * **extendedLayoutIncludesOpaqueBars**
> 
If your design uses opaque bars, refine `edgesForExtendedLayout` by also setting the `extendedLayoutIncludesOpaqueBars` property to **NO**. (The default value of `extendedLayoutIncludesOpaqueBars` is **NO**.)

> * **automaticallyAdjustsScrollViewInsets**
> 
If you don’t want a scroll view’s content insets to be automatically adjusted, set `automaticallyAdjustsScrollViewInsets` to NO. (The default value of `automaticallyAdjustsScrollViewInsets` is YES.)

> * **topLayoutGuide, bottomLayoutGuide**
> 
The `topLayoutGuide` and `bottomLayoutGuide` properties indicate the location of the top or bottom bar edges in a view controller’s view. If bars should overlap the top or bottom of a view, you can use Interface Builder to position the view relative to the bar by creating constraints to the bottom of `topLayoutGuide` or to the top of `bottomLayoutGuide`. (If no bars should overlap the view, the bottom of  `topLayoutGuide` is the same as the top of the view and the top of  `bottomLayoutGuide` is the same as the bottom of the view.) Both properties are lazily created when requested.

我自己的理解：在iOS7中视图控制器都使用了`full-screen layout(全屏布局)`，而且全屏布局细化到了每个边缘的布局。而`wantsFullScreenLayout`在iOS7中废弃了。不过可以用`edgesForExtendedLayout`、`extendedLayoutIncludesOpaqueBars`、`automaticallyAdjustsScrollViewInsets`、`topLayoutGuide，bottomLayoutGuide`这几个属性来适配控制器的布局。
