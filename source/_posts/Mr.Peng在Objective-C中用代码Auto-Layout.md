title: Mr.Peng在Objective-C中用代码Auto-Layout
date: 2015-07-10 16:14:53
categories: [Moblie]
tags:
- Objective-C
- Demo
- Auto Layout

---
##简介
Auto Layout在2012年随着iOS6一起发布，至今已经快3年了。而我自己工作经验快两年，最近才开始接触用代码Auto Layout，的确有点不好意思（不过之前使用的都是xib和storyboard）。其实现在已经有很多对Auto Layout进行封装的三方轻框架，使用起来还比较方便，快捷。比如：
> * [Masonry](https://github.com/SnapKit/Masonry)
> * [PureLayout](https://github.com/smileyborg/PureLayout)
> * [FLKAutoLayout](https://github.com/floriankugler/FLKAutoLayout)
> * [KeepLayout](https://github.com/iMartinKiss/KeepLayout)


原文地址：[http://www.mrpeng.me/2015/07/10/Objective-C%E4%B8%AD%E7%94%A8%E4%BB%A3%E7%A0%81Auto-Layout/](http://www.mrpeng.me/2015/07/10/Objective-C%E4%B8%AD%E7%94%A8%E4%BB%A3%E7%A0%81Auto-Layout/)

但是对于初次使用Auto Layout的我来说，工具始终是用来辅助，让我提高效率的，但是知识是一定要自己去了解和学习的，所以我觉得应该搞懂最基本，才能说我要用工具来辅助我，让我效率提高。

<!--more-->

##Auto Layout
个人理解：`Auto Layout`就是利用对上、下、左、右的约束条件来控制视图的相对位置。
官方解释：
> `Auto Layout` 是一个系统，可以让你通过创建元素之间关系的数学描述来布局应用程序的用户界面。——[《Auto Layout Guide》](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010853-CH13-SW1)
> 
> `Auto Layout` 是一种基于约束的，描述性的布局系统。——[《Taking Control of Auto Layout in Xcode 5 - WWDC 2013》](https://developer.apple.com/videos/wwdc/2013/#406)

Auto Layout中有4个关键字：
> * 元素
> * 关系
> * 约束
> * 描述


###元素
可以从数组中去理解这个关键字,比如`@"3"`,`@"4"`是`@[@"3",@"4",@"5"]`中的元素。或者从键盘中去理解，键盘里面的每个按键即是键盘的每个元素。在视图中的话，子视图就是一个元素。
###关系(Relation)
`关系`即是元素之间关系。还是用键盘举例：`Z`键和`X`键之间有关系，他们关系：`Z`键和`X`键宽度、高度一样，`Z`键在`X`键的`左边`，`X`键在`Z`键的`右边`，`Z`键和`X`键之间`距离相差多少`等等。。
###约束(Constraint)
元素之间关系的限制就是约束。约束是Auto Layout中最重要的概念。上面提及的`左边`，`右边`，`距离相差多少`这些都是约束，限制了元素之间的关系。
###描述
定义约束来显示元素之间的关系。继续用键盘举例，`Z`键的长宽均为`1厘米`，`左边`距离键盘的左边缘`15厘米`，`上边`距离键盘的顶部`8厘米`。这句话就可以定位`Z`键在键盘中的位置，很轻松就可以计算出`Z`键的`frame`为`{15.0,8.0,1.0,1.0}`。
现在`Z`键坐标确定了，那么`X`键坐标就可以相对于`Z`键来描述：顶部和`Z`键对齐，大小和`Z`键相等，位于`Z`键右侧`0.5厘米`处，就可以直接计算出`X`键的`frame`了。其实这句话中包含了元素间的关系，关系间的约束。
##如何在代码中使用Auto Layout
无论在Xib中还是代码中使用Auto Layout，你都需要了解Auto Layout的一些必要知识。Auto Layout中约束对应的类是`NSLayoutConstraint`，一个`NSLayoutConstraint`实例代表一条约束。
###NSLayoutConstraint
方法有两个：
####第一个：

```
+ (id)constraintWithItem:(id)view1
               attribute:(NSLayoutAttribute)attribute1
               relatedBy:(NSLayoutRelation)relation
                  toItem:(id)view2
               attribute:(NSLayoutAttribute)attribute2
              multiplier:(CGFloat)multiplier
                constant:(CGFloat)constant;
```
> * `view1`就是你需要操作的视图，可以理解为键盘的`Z`键；
> * `attribute1`就是`view1`中需要操作的约束，有上、下、左、右等，可以用`⌘ + 左键`点击`NSLayoutAttribute`查看具体的约束条件；
> * `relation`就是`view1`和`view2`约束之间的关系，可以用`⌘ + 左键`点击`NSLayoutRelation`查看具体的介绍，比如`NSLayoutRelation`中的`NSLayoutRelationEqual`就是表示`view1`和`view2`约束之间的关系相等。
> * `view2`就是目标视图，可以理解为整个键盘的大小；
> * `attribute2`就是`view2`中约束，
> * `multiplier`就是`attribute1`和`attribute2`的比例，比如`multiplier:0.8`表示所设置的`attribute1`是`attribute2`的0.8倍;
> * `constant`可以理解为偏移量。

可以简单的理解为下面公式：

```
	view1.attribute1 = view2.attribute2 × multiplier + constant
```
####实践
本人就简单的介绍下，如果用Auto Layout在屏幕上均等分布4个ImageView如下图所示：
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/10iOS%20Simulator%20Screen%20Shot%202015年7月10日%20下午3.03.01.png)
代码如下：

```
	UIImageView *view = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"1"]];
    view.contentMode = UIViewContentModeScaleAspectFit;
    [self.view addSubview:view];
    view.backgroundColor = [UIColor blueColor];
    
    UIImageView *view1 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"2"]];
    [self.view addSubview:view1];
    view1.contentMode = UIViewContentModeScaleAspectFit;
    view1.backgroundColor = [UIColor yellowColor];
    
    UIImageView *view2 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"3"]];
    [self.view addSubview:view2];
    view2.contentMode = UIViewContentModeScaleAspectFit;
    view2.backgroundColor = [UIColor greenColor];
    
    UIImageView *view3 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"4"]];
    [self.view addSubview:view3];
    view3.contentMode = UIViewContentModeScaleAspectFit;
    view3.backgroundColor = [UIColor cyanColor];
    
    [view setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view1 setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view2 setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view3 setTranslatesAutoresizingMaskIntoConstraints:NO];
    
    //<-------------设置View------------->
    /**
     *  View的左边距和self.view的左边距一样
     */
    NSLayoutConstraint *viewXConstraint = [NSLayoutConstraint constraintWithItem:view attribute:NSLayoutAttributeLeft relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeLeft multiplier:1 constant:0.0];
    /**
     *  view的中心Y坐标和self.view的中心Y坐标一样
     */
    NSLayoutConstraint *viewYConstraint = [NSLayoutConstraint constraintWithItem:view attribute:NSLayoutAttributeCenterY relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeCenterY multiplier:1 constant:0.0];
    /**
     *  view的宽度等于self.view宽度的1/4即0.25
     */
    NSLayoutConstraint *viewWidthConstraint = [NSLayoutConstraint constraintWithItem:view attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeWidth multiplier:0.25f constant:0.0f];
    /**
     *  由于UIImageView设置了图片，有了高度，就直接使用view自身的高度，当然上面的宽度也可以使用图片的宽度。
     */
    NSLayoutConstraint *viewHeightConstraint = [NSLayoutConstraint constraintWithItem:view attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationEqual toItem:view attribute:NSLayoutAttributeHeight multiplier:1 constant:0.0f];
    if ([[UIDevice currentDevice].systemVersion doubleValue] < 8.0f) {
        viewXConstraint.active = YES;
        viewYConstraint.active = YES;
        viewWidthConstraint.active = YES;
        viewHeightConstraint.active = YES;
    } else {
        [self.view addConstraints:@[viewXConstraint,
                                   viewYConstraint,
                                   viewWidthConstraint,
                                   viewHeightConstraint]];
    }
    
    //<-------------设置View1------------->
    [self.view addConstraints:@[
                                //view1的x坐标等于view的最右边
                                [NSLayoutConstraint constraintWithItem:view1
                                                             attribute:NSLayoutAttributeLeft
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view
                                                             attribute:NSLayoutAttributeRight
                                                            multiplier:1
                                                              constant:0.0],
                                //view1的中心Y坐标等于View的中心Y坐标
                                [NSLayoutConstraint constraintWithItem:view1
                                                             attribute:NSLayoutAttributeCenterY
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view
                                                             attribute:NSLayoutAttributeCenterY
                                                            multiplier:1
                                                              constant:0.0],
                                //view1的宽度等于View的宽度
                                [NSLayoutConstraint constraintWithItem:view1
                                                             attribute:NSLayoutAttributeWidth
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view
                                                             attribute:NSLayoutAttributeWidth
                                                            multiplier:1
                                                              constant:0.0],
                                //view1的高度等于View的高度
                                [NSLayoutConstraint constraintWithItem:view1
                                                             attribute:NSLayoutAttributeHeight
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view
                                                             attribute:NSLayoutAttributeHeight
                                                            multiplier:1
                                                              constant:0.0],
                               
                                ]];
    //<-------------设置View2------------->
    [self.view addConstraints:@[
                                //view2的x坐标等于view1的最右边
                                [NSLayoutConstraint constraintWithItem:view2
                                                             attribute:NSLayoutAttributeLeft
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view1
                                                             attribute:NSLayoutAttributeRight
                                                            multiplier:1
                                                              constant:0.0],
                                //view2的中心Y坐标等于View1的中心Y坐标
                                [NSLayoutConstraint constraintWithItem:view2
                                                             attribute:NSLayoutAttributeCenterY
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view1
                                                             attribute:NSLayoutAttributeCenterY
                                                            multiplier:1
                                                              constant:0.0],
                                //view2的宽度等于View1的宽度
                                [NSLayoutConstraint constraintWithItem:view2
                                                             attribute:NSLayoutAttributeWidth
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view1
                                                             attribute:NSLayoutAttributeWidth
                                                            multiplier:1
                                                              constant:0.0],
                                //view2的高度等于View1的高度
                                [NSLayoutConstraint constraintWithItem:view2
                                                             attribute:NSLayoutAttributeHeight
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view1
                                                             attribute:NSLayoutAttributeHeight
                                                            multiplier:1
                                                              constant:0.0],
                                
                                ]];
    //<-------------设置View3------------->
    [self.view addConstraints:@[
                                //view3的x坐标等于view2的最右边
                                [NSLayoutConstraint constraintWithItem:view3
                                                             attribute:NSLayoutAttributeLeft
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view2
                                                             attribute:NSLayoutAttributeRight
                                                            multiplier:1
                                                              constant:0.0],
                                //view3的中心Y坐标等于View2的中心Y坐标
                                [NSLayoutConstraint constraintWithItem:view3
                                                             attribute:NSLayoutAttributeCenterY
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view2
                                                             attribute:NSLayoutAttributeCenterY
                                                            multiplier:1
                                                              constant:0.0],
                                //view3的宽度等于View2的宽度
                                [NSLayoutConstraint constraintWithItem:view3
                                                             attribute:NSLayoutAttributeWidth
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view2
                                                             attribute:NSLayoutAttributeWidth
                                                            multiplier:1
                                                              constant:0.0],
                                //view3的高度等于View2的高度
                                [NSLayoutConstraint constraintWithItem:view3
                                                             attribute:NSLayoutAttributeHeight
                                                             relatedBy:NSLayoutRelationEqual
                                                                toItem:view2
                                                             attribute:NSLayoutAttributeHeight
                                                            multiplier:1
                                                              constant:0.0],
                                
                                ]];
```
每条语句，我都在代码中有所注释。看上去是不是感觉很繁琐呀。如果有心的读者肯定会有疑问了，上面不是说的有两种方法么？目前为止只说了一种。对的，上面就是针对第一个方法做出了对应的实践。下面介绍下第二种比较简洁的方法：
####第二种方法：

```
+ (NSArray *)constraintsWithVisualFormat:(NSString *)format 
                                 options:(NSLayoutFormatOptions)opts 
                                 metrics:(NSDictionary *)metrics 
                                   views:(NSDictionary *)views;
```
> * `format`是使用[可视化格式语言(VFL)](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1)编写的，存放布局逻辑；
> * `opts`和上面那个方法中的`NSLayoutAttribute`类似，也是限定上、下、左、右等约束；
> * `metrics`就是传入的一个size或者orign甚至是frame，根据这个参数来在`format`进行逻辑布局；
> * `views`包含了操作视图和目标视图。

还是以上面的例子，进行实践，代码如下：

```
	
	UIImageView *view = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"1"]];
    view.contentMode = UIViewContentModeScaleAspectFit;
    [self.view addSubview:view];
    view.backgroundColor = [UIColor blueColor];
    
    UIImageView *view1 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"2"]];
    [self.view addSubview:view1];
    view1.contentMode = UIViewContentModeScaleAspectFit;
    view1.backgroundColor = [UIColor yellowColor];
    
    UIImageView *view2 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"3"]];
    [self.view addSubview:view2];
    view2.contentMode = UIViewContentModeScaleAspectFit;
    view2.backgroundColor = [UIColor greenColor];
    
    UIImageView *view3 = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"4"]];
    [self.view addSubview:view3];
    view3.contentMode = UIViewContentModeScaleAspectFit;
    view3.backgroundColor = [UIColor cyanColor];
    
    [view setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view1 setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view2 setTranslatesAutoresizingMaskIntoConstraints:NO];
    [view3 setTranslatesAutoresizingMaskIntoConstraints:NO];
    
    NSDictionary *views = NSDictionaryOfVariableBindings(self.view, view, view1,view2, view3);
    
    //计算四分之一宽度
    float quarterWidth = self.view.frame.size.width / 4;
    //获取图片高度
    float height = view.image.size.height;
    
    NSDictionary *metrics = @{@"width":[NSNumber numberWithFloat:quarterWidth],@"height":[NSNumber numberWithFloat:height]};
    
    //view 水平方向：离self.view的左边距为0.
    [self.view addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[view(==width)]" options:0 metrics:metrics views:views]];
    
    /* view的中心Y坐标等于self.view的中心Y坐标。
     * 这里因为用到了具体的宽度和高度，如果用VFL来设置居中的话，会报错的。只能辅助用第一种方法来解决此问题。
     */
    NSLayoutConstraint *viewYConstraint = [NSLayoutConstraint constraintWithItem:view attribute:NSLayoutAttributeCenterY relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeCenterY multiplier:1 constant:0.0];
    [self.view addConstraint:viewYConstraint];
    
    //设置view和view1、view1和view2、view2和view3间隔为0。其中 -0- 可以省去。
    [self.view addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"H:[view]-0-[view1(==view)]-0-[view2(==view1)]-0-[view3(==view2)]" options:NSLayoutFormatAlignAllBottom|NSLayoutFormatAlignAllTop metrics:nil views:views]];
```

###Demo
[https://github.com/pengrun/AutoLayoutDemo](https://github.com/pengrun/AutoLayoutDemo)

###总结
从代码简洁度来说，是不是用第二种方法样简洁些，但是如果说逻辑清晰的话，还是建议用第一种。不过在具体的开发中，还要根据自己项目的实际情况来协同合作。

参考文章：
[iOS 开发实践之 Auto Layout](http://xuexuefeng.com/autolayout/)
[IOS Autolayout(VFL) 处理子视图居中](http://blog.csdn.net/chujiujiao/article/details/13020813)
[IOS页面自动布局 之 NSLayoutConstraint基础篇](http://www.cnblogs.com/xieyajie/p/4612697.html)
