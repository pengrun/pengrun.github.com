title: 'Swift 2.0: Session 1 学习'
date: 2015-06-23 11:24:02
categories: [Mobile]
tags:
- Swift
- 翻译

---
##申明
我**不是**一个**专业翻译人员**，肯定会有**翻译理解有误**的地方，很多**不确定**的都把**原文**贴出来了，附上了**自己的理解**，所以以下内容有哪里翻译有误请大家指出。

翻译来源：iBook中搜索 Swift 下载 [The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)
原文地址：[http://www.mrpeng.me/2015/06/23/Swift-2-0-Session-1-%E5%AD%A6%E4%B9%A0/](http://www.mrpeng.me/2015/06/23/Swift-2-0-Session-1-%E5%AD%A6%E4%B9%A0/)

##摘要
在iBook上面下载了[The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)，决定跟着这本书自学Swift 2.0，而且还有个冲动的想法，那就是把这本书自己翻译出来，当然自己英语水平有限，肯定很多地方翻译不到位，希望大家喷浅点。

<!--more-->

##欢迎来到Swift
###关于Swift
Swift在iOS，OS X中是一门新编程语言，watchOS apps呢，最好以C 和 Objective-C来创建，Swift没有限制C语言的兼容性了。Swift 采用安全编程模式并且新增的最新特征让编程更容易，更`flexible(灵活)`，更有趣。Swift `clean slate(底层)`依靠成熟并且友好的Cocoa 和 Cocoa Touch框架，可选择的去`reimagine(重新构思)`软件开发的工作原理。

其实，Swift已经开发了有很多年了。Apple通过提高已有的开发师，测试者师，架构师，就为了给Swift打下基础。我们通过ARC来简化内存管理。我们的框架建立在Foundation 和 Cocoa稳定的基础之上，而且已经现代化、标准化了。Objective-C已经进化成可以支持blocks，collection，模块化，`enabling framework adoption of modern language technologies without disruption（有利的框架采用没有中断的最新科学语言）`。很感谢这些基础工作的人们，才使得现在，我们能够介绍一门为未来Apple的软件开发的新语言。

Swift对Objective-C开发者来说是非常亲密。它采用了Objective-C声明参数的可读性和动态建模的能力。它提供无缝访问现有Cocoa框架和混合匹配了Objective-C代码的互通性。从这些共同点进行建立，Swift `introduce（引出）`大量新特征和统一的程序还有面向对象。

Swift对于菜鸟程序员来说是很好学习的。能够做为体验和享受的脚本语言，使它成为第一个工业质量体系编程语言。能够支持有创新特征的playrounds，playrounds能够让程序员尝试用Swift编码并且不用再去building和running项目就能够立刻看到结果。

`Swift combines the best in modern language thinking with wisdom from the wider apple engineering culture(Swift从更广泛apple工程师文化中结合出了一门最好充满智慧想法的现代语言)`。编译器对性能进行了优化，语言对开发进行了优化，`without compromising on either（也没有原有的性能和语言特性）`。它的设计从“hello,world”到整个操作系统。`All this makes Swift a sound future investment for developers and for Apple(这一切都让我们听到Swift是开发者和apple公司未来投资的声音)`。

用Swift来写iOS，OS X和watchOS apps是一种非常棒的方式，并且Swift将继续跟进新的特性和性能。我们对Swift的发展是信心满满的。我们迫不及待的想看到你如何使用它。