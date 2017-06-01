title: Mr.Peng学习KVC、KVO
date: 2015-09-25 15:42:00
categories: [Moblie]
tags:
- KVO、KVC
- iOS9

---
## 简介
由于`Objective-C`主要基于Smalltalk进行设计的，因此基于很多类似于`Ruby`、`Python`的动态特性，例如动态类型、动态加载、动态绑定等。`Objective-C`中的`KVC`与`KVO`是两种比较重要的技术。所以这篇文件着重介绍`键值编码（KVC）、键值监听（KVO）`特性。

原文地址：[http://www.mrpeng.me/2015/09/25/Mr-Peng学习KVC、KVO/](http://www.mrpeng.me/2015/09/25/Mr-Peng学习KVC、KVO/)


## 键值编码KVC
### 概述

我们知道在C#中可以反射读写一个对象的属性，有时候这种方式特别方便，因为你可以利用字符串的方式去动态控制一个对象。其实由于`Objective-C`的语言特性，你根本不必进行任何操作就可以进行属性的动态读写，这种方式就是`Key Value Coding（KVC）`。它是一种可以直接通过字符串的名字(key)来访问`类属性(实例变量)`的机制。而不是通过调用`Seeter`、`Getter`方法访问。

<!--more-->

### 使用方法
`KVC`的操作方法有`NSKeyValueCoding`协议提供，而`NSObject`就实现了这个协议，也就是说`Objective-C`中几乎所有对象都支持`KVC`操作，例如：你是否在`github`上下载的Demo中有人用`Dictionary`时，不是用`objectForKey`而使用`valueForkey`呢，这就是一种`KVC`。常用的`KVC`操作方法如下：

*	动态设置：
	*	`setValue:属性值 forKey:属性名`(用于简单路径)；
	*	`setValue:属性值 forKeyPath:属性路径`(用于复合路径，例如Car有一个Brand类型属性，那么car.brand就是一个复合属性)；
	*	`setValue:属性值 forUndefinedKey:未声明属性名`；
	*	`setNilValueForKey:属性名`(当对非类对象属性设置nil时调用，默认跑出异常)；
*	动态读取：
	*	`valueForKey:属性名`；
	*	`valueForKeyPath:属性名`(用于复合路径)；
	*	`valueForUndefinedKey:为声明属性名`(它的默认实现是抛出异常，可以重写这个函数做错误处理)

### 实例
Brand.h

	//
	//  Brand.h
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import <Foundation/Foundation.h>

	@interface Brand : NSObject
	@property (nonatomic, strong) NSString *name;
	@end

Car.h

	//
	//  Car.h
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import <Foundation/Foundation.h>
	@class Brand;
	@class Series;
	@interface Car : NSObject
	{
    	@private
    	/**
     	*  车架号
     	*/
    	NSString *_VinCode;
	}
	/**
	 *  类型
 	*/
	@property (nonatomic, strong) NSString *type;
	/**
 	*  品牌
 	*/
	@property (nonatomic, strong) Brand *brand;
	/**
 	*  系列
 	*/
	@property (nonatomic, strong) Series *series;

	- (void)showCarInfo;
	@end
Car.m

	//
	//  Car.m
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import "Car.h"
	#import "Series.h"
	@implementation Car
	- (void)showCarInfo {
    NSLog(@"CarType = %@,CarVinCode = %@",_type,_VinCode);
	}
	@end
	
ViewController.m

	//
	//  ViewController.m
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import "ViewController.h"
	#import "Car.h"
	#import "Brand.h"
	@interface ViewController ()

	@end

	@implementation ViewController

	- (void)viewDidLoad {
   		[super viewDidLoad];
    
    	[self showKVCExample];
    	// Do any additional setup after loading the view, typically from a nib.
	}

	- (void)showKVCExample {
    	Car *car = [[Car alloc] init];
    	[car setValue:@"Suv" forKey:@"type"];
    	[car setValue:@"20150925" forKey:@"VinCode"]; //VinCode为私有变量，但是仍然可以访问
    	[car showCarInfo];
    	//结果：CarType = Suv,CarVinCode = 20150925
    
    	NSLog(@"KVC------- CarType is :%@,CarVinCode is :%@",car.type,[car valueForKey:@"VinCode"]);
    	//结果：KVC------- CarType is :Suv,CarVinCode is :20150925
    
    	Brand *brand = [[Brand alloc] init];
    	car.brand = brand;
    	[car setValue:@"路虎" forKeyPath:@"brand.name"];
    	NSLog(@"KVC------- car's brand is :%@",[car valueForKeyPath:@"brand.name"]);
		//结果：KVC------- car's brand is :路虎
    
	}
	@end
### KVO总结
KVC使用起来比较简单，但是它如何查找一个属性进行读取呢？具体查找规则（假设现在要利用KVC对a进行读取）：

*	如果是动态设置属性，则优先考虑调用setA方法，如果没有该方法则优先考虑搜索成员变量_a,如果仍然不存在则搜索成员变量a，如果最后仍然没搜索到则会调用这个类的setValue:forUndefinedKey：方法(注意搜索过程中不管这些方法、成员变量是私有的还是公共的都能正确设置)；
*	如果是动态读取属性，则优先考虑调用a方法（属性a的getter方法），如果没有搜索到则会优先搜索成员变量_a，如果仍然不存在则搜索成员变量a，如果最后仍然没搜索到则会调用这个类的valueforUndefinedKey:方法(注意搜索过程中不管这些方法、成员变量是私有的还是公共的都能正确读取)；

## 键值监听KVO
### 概述
我们知道在`WPF`、`Silverlight`中都有一种双向绑定机制，如果数据模型修改了之后会立即反应到UI视图上，类似的还有如今比较流行的基于MVVM设计模式的前端框架。其实在`Objective-C`中原生就支持这种机制，它叫做：`Key Value Observing（简称KVO）`。`KVO`其实是一种观察者模式，利用它可以很容易实现视图组件和数据模型的分离，当数据模型的属性值改变之后作为监听器的视图组件就会被激发，激发时就会回调监听器自身。在`Objective-C`中要实现`KVO`则必须实现`NSKeyValueObsering`协议，不过幸运的是`NSObject`已经实现了该协议，因此几乎所有的`Objective-C`对象都可以使用`KVO`。
### 使用方法
在`Objective-C`中使用`KVO`操作常用的方法如下：

*	注册指定Key路径监听器：`addObserver:forKeyPath:option:context:`
*	删除指定Key路径监听器：`removeObserver：forKeyPath`、`removeObserver:forKeyPath:context:`
*	回调监听：`observerValueForKeyPath:ofObject:change:context:`

`KVO`的使用步骤也比较简单：

1.	通过`addObserver:forKeyPath:options:context:`注册监听器；
2.	重写监听器的`observeValueForKeyPath:ofObject:change:context:`方法；

### 实例
在上面的例子基础上继续扩展，假设当`Car`的`Series`变动之后我们希望`Car`及时获取通知。那么此时`Series`就作为被监听的对象，需要`Car`为它注册监听（使用`addObserver:forKeyPath:context:`）;
Series.h

	//
	//  Series.h
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import <Foundation/Foundation.h>

	@interface Series : NSObject
	@property (nonatomic, strong) NSString *name;
	@end
Car.h

	//
	//  Car.h
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import <Foundation/Foundation.h>
	@class Brand;
	@class Series;
	@interface Car : NSObject
	{
    	@private
    	/**
    	 *  车架号
    	 */
    	NSString *_VinCode;
	}
	/**
 	*  类型
 	*/
	@property (nonatomic, strong) NSString *type;
	/**
 	*  品牌
 	*/
	@property (nonatomic, strong) Brand *brand;
	/**
 	*  系列
 	*/
	@property (nonatomic, strong) Series *series;

	- (void)showCarInfo;
	@end
Car.m

	//
	//  Car.m
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import "Car.h"
	#import "Series.h"
	@implementation Car
	- (void)showCarInfo {
 	   NSLog(@"CarType = %@,CarVinCode = %@",_type,_VinCode);
	}

	- (void)setSeries:(Series *)series {
    	_series = series;
    	[self.series addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew context:nil];
	}

	- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context {
   		if ([keyPath isEqualToString:@"name"]) {
        	NSLog(@"keyPath=%@,object=%@,newValue=%@,context=%@",keyPath,object,[change objectForKey:@"new"],context);
    	}
	}

	- (void)dealloc {
    	[self.series removeObserver:self forKeyPath:@"name"];
	}
	@end
	
ViewController.m

	//
	//  ViewController.m
	//  PRKVOKVCDemo
	//
	//  Created by MH-Pengrun on 15/9/25.
	//  Copyright © 2015年 MH. All rights reserved.
	//

	#import "ViewController.h"
	#import "Car.h"
	#import "Brand.h"
	#import "Series.h"
	@interface ViewController ()

	@end

	@implementation ViewController

	- (void)viewDidLoad {
    	[super viewDidLoad];
    
    	[self showKVOExample];
    	// Do any additional setup after loading the view, typically from a nib.
	}
	- (void)showKVOExample {
    	Car *car = [[Car alloc] init];
    	car.type = @"Suv";
    	Series *series = [[Series alloc] init];
    	series.name = @"揽胜";
    	car.series = series;
    	NSLog(@"KVO------- series.name:%@",series.name);
    	NSLog(@"KVO------- car.series.name:%@",car.series.name);
    	series.name = @"极光";
    
	}

	- (void)didReceiveMemoryWarning {
    	[super didReceiveMemoryWarning];
    	// Dispose of any resources that can be recreated.
	}

	@end
### KVO总结
`KVO`这种编码方式使用起来很简单，很适合与`dataModel`修改之后，引发的`UIView`的变化这种情况，就像以上例子，当更改属性的值之后，监听对象会立即得到通知。

