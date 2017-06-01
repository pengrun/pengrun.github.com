title: Mr.Peng推荐Objective-C中Storyboard小技巧(适合xib)
date: 2015-07-21 10:33:47
categories: [Moblie]
tags:
- Objective-C
- Storyboard

---
#简介
原文地址：[http://www.mrpeng.me/2015/07/21/Storyboadr%E5%B0%8F%E6%8A%80%E5%B7%A7-%E9%80%82%E5%90%88xib/](http://www.mrpeng.me/2015/07/21/Storyboadr%E5%B0%8F%E6%8A%80%E5%B7%A7-%E9%80%82%E5%90%88xib/)

由于以前的工作比较忙而且项目比较急，都没时间去用新东西去写项目，就由于开车时，走一条自己熟悉路，总比别人推荐的近路要近一些。最近由于换工作了，压力相对要小，所以有时间去学习新东西，而对于我来说新东西就是Storyboard，希望高手勿喷哈。本文呢，就是介绍下在Storyboard中的一些小技能，当然很多都在网上搜集的，我只是进行归纳一下。
#Storyboard
##利用Storyboard设置圆角、边框、边框颜色
###圆角
由于很多项目中，需要通过设置圆角来美化UI，而很多人都知道利用代码来设置圆角：

<!--more-->

````
	self.view.layer.cornerRadius = 5;	//圆角
	self.view.layer.borderWidth = 1;	//边框宽度
	self.view.layer.borderColor = [UIColor redColor].CGColor; //边框颜色
````
如果我们利用StoryBoard然后又通过拖动outlet来设置圆角的话，相对来说有点麻烦了。那么我们就打开Storyboard选中一个`View`然后点击`右侧`的`Show the identity inspector`如下图：
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/21.png)
>* `layer.cornerRadius`是设置`圆角`,注：网上有些高手说`layer.corner.radius`来设置圆角，我自己的测试哈，是达不到圆角效果的;
>* `layer.borderWidth`是设置`边框宽度`;
>* `layer.borderColorFromUIColor`是设置`边框颜色`;

如果不细心的朋友，用了上面方法设置`边框宽度`和`边框颜色`之后，肯定会在心里骂我，尼玛，说好的边框颜色呢，不管怎么设置为何都是黑色。表急，我们在代码中设置边框颜色是通过`CGColor`来的，而我们在`Key path`中只是设置了UIColor，所以系统就会默认我们的边框颜色是黑色。那如何解决此问题呢？
####思路
利用`Category`为`CALayer`添加一个UIColor属性。
####代码
````
#import <QuartzCore/QuartzCore.h>
#import <UIKit/UIKit.h>
@interface CALayer (Additions)
@property(nonatomic, strong) UIColor *borderColorFromUIColor;
- (void)setBorderColorFromUIColor:(UIColor *)color;
@end

----------------------------------------------------------------------------------------

#import "CALayer+Additions.h"
#import <objc/runtime.h>
@implementation CALayer (Additions)
- (UIColor *)borderColorFromUIColor {
    return objc_getAssociatedObject(self, @selector(borderColorFromUIColor));
}

-(void)setBorderColorFromUIColor:(UIColor *)color
{
    objc_setAssociatedObject(self, @selector(borderColorFromUIColor), color, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    
    [self setBorderColorFromUI:self.borderColorFromUIColor];
}

- (void)setBorderColorFromUI:(UIColor *)color

{
    self.borderColor = color.CGColor;
}
@end
````
###效果
这样就达到了我们想要的效果了。如下图：
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/212.png)

##在Storyboard中为试图添加辅助线
辅助线在设计中是很重要的，为了我们布局更加准确，必须需要辅助线来助我一臂之力。
###增加辅助线
####操作
选中某个`view`
> * 按下`shift`+`⌘`+`-`增加水平辅助线
> * 按下`shift`+`⌘`+`|`增加垂直辅助线
所增加的辅助线都是水平、垂直居中的。
####效果
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/213.png)
###移动辅助线
####操作
光标移动到线上会出现可拖动的标识。
####效果
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/214.png)
###删除辅助线
####操作
由于删除断点一样，拖动辅助线到view视图之外，就会删除了。
###其他功能
还有一些功能，比如
> * 根据文字内容来适配`label`或者`button`的`宽`、`高`；
> * 显示`Layout`、`Bounds`的`rectAngles`；
> * 显示`Constraint`
等等，都通过选中`storyboard`后在`菜单栏`中选中`Editor`->`Canvas` 里面有很多功能和附带的快捷键。
![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015/07/215.png)

#结语
不得不佩服`xcode`真的很强大。
