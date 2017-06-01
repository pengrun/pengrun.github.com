## Mr.Peng教你正确使用R.Swift

title: 'Mr.Peng教你正确使用R.Swift'

categories: [Moblie]
tags:

- XCode 8
- iOS 10
- R.Swfit
- Swift

------

## 前言

由于XCode8把插件屏蔽了，以前用得得心应手的[https://github.com/ksuther/KSImageNamed-Xcode](https://github.com/ksuther/KSImageNamed-Xcode)插件无法使用了，最近学习`Swift`发现了一个神器[R.swift](https://github.com/mac-cain13/R.swift)，以一种优雅安全的方式使用资源文件，不管是图片，各种资源型的引用均可用它来完成。

<!--more-->

## 作用

在iOS开发中，资源的调用方面一直是不太严谨的。 比如说，我们要在代码中初始化一个 UIImage。那么通常的做法是：
`let image = UIImage(named: "MrPeng_TestImage")`
传入的参数是以`字符串`的方式传入，那么这个字符串便是我们要讨论的不严谨的地方，它有哪些风险呢？

> 1.编写代码时不一小心手误，写错了（当时的你可能不知道）。
> 2.根据项目情况，资源增多，需要对资源的名称重写整理和维护，也就是要修改。
> 3.这个资源已经不再需要使用，可以删除了。

那么遇到上面讲到的情况，我们就需要对曾经写过的代码进行对应的检查和修正。但是，这需要开发者自己养成好习惯，才能防止一个新的BUG产生，或者是一段没用得代码被编译。

而[R.swift](https://github.com/mac-cain13/R.swift)的出现彻底解决了资源调用不严谨的问题，通过 R.swift 生成的代码在Xcode编译时即可检查出资源使用上是否存在安全隐患。为此，你必须修正你的代码，否则，你是编译不过去的！

## R.swift

R.swift 是一个Mac上面终端程序，采用Swift编写并作用于Swift，R.swift 最终会生成一个名字叫做 R.generated.swift 的类文件。
它是一个强类型资源调用工具，也是我们这篇文章的主角！

从前，你需要这样编写资源调用的相关代码：

```
let icon = UIImage(named: "settings-icon")
let cell = tableView.dequeueReusableCellWithReuseIdentifier("textCell", forIndexPath: indexPath) as? TextCell
performSegueWithIdentifier("openSettings")
```

使用R.swift，你不需要再写冒号`“`了,最关键的是----全部都有代码提示。

```
let icon = R.image.settingsIcon
let cell = tableView.dequeueReusableCellWithReuseIdentifier(R.reuseIdentifier.textCell, forIndexPath: indexPath)
performSegueWithIdentifier(R.segue.openSettings)
```

## 安装

**强烈建议使用CocoaPods**

1. 添加 'R.swift' 到你的pod file

   ```source &#39;https://github.com/CocoaPods/Specs.git&#39;
   platform :ios, '8.0'
   use_frameworks!
   target 'RSwiftTest' do
       pod 'R.swift'
   end
   ```

2. 然后 `pod install`

3. 选择项目配置，点击左上角的小加号，添加一个`New Run Script Phase`

![](http://ww4.sinaimg.cn/large/006tKfTcgy1ffqqgfwrm1j30sj09nmz1.jpg)

4. 将新添加的Script的顺序拖动到compile Sources前面

**这步非常重要！**

拖动后顺序是这样

![](http://ww1.sinaimg.cn/large/006tNc79gy1ffqqhe2lg7j30np09ljsd.jpg)

5. 在run script中贴进下面的代码：`"$PODS_ROOT/R.swift/rswift" "$SRCROOT"`

   ![](http://ww4.sinaimg.cn/large/006tNc79gy1ffqqk9dw05j30ks0bu3zb.jpg)

6. build完成后，会在项目的根目录下发现`R.generated.swift`这个文件，将它添加进项目。![](http://ww2.sinaimg.cn/large/006tNc79gy1ffqqnua3e7j305v058dg8.jpg)

   但是在`Copy items if needed`不要勾选![](http://ww1.sinaimg.cn/large/006tNc79gy1ffqqozl2qzj30ka0bywff.jpg)

## 测试

![](http://ww2.sinaimg.cn/large/006tNc79gy1ffqqscnc6dj30s60b9n08.jpg)

## 完结

很多教程均是网上搜到的，加上自己遇到的问题总结出的此文章。