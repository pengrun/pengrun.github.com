## Mr.Peng iOS Charts for Objective-C with Podcocoa 集成步骤

title: 'Mr.Peng iOS Charts for Objective-C with Podcocoa 集成步骤'

categories: [Moblie]
tags:

- XCode 8
- iOS 10
- swift
- Objective-C

------

### 前言

目前OC上面的图表可能无法满足各产品经理的需求了，而在iOS界目前最强大的就是`Charts`了，而`Charts`呢又是Swift版的，并且又是推荐使用`carthage`，那么实用Podcocoa的OC是否感觉很尴尬呢。最近有一位朋友就遇到此问题，前来想问咨询，通过自己的洪荒之力，终于搞定了。记录下步骤和大家分享一下。

### 前提

1. 已经默认您装好了Podcocoa；
2. 并且已经创建了`Objective-C`工程；

<!--more-->

### 步骤

1. 关闭工程，在命令行中 cd 工程目录，回车，终端显示如下图![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9zpr61e3sj30fu00zglt.jpg)

2. 新建一个Podfile文件 

   ```touch Podfile
   $ touch Podfile
   ```

   终端显示如下图：![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9zpth7i2uj309t00hdfw.jpg)

3. 查找`Charts`

   ``` $ pod search charts
   $ pod search charts
   ```

   可能会等待几分钟，最终出现如下图：![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9zpzehaqxj30fd096q4m.jpg)

   复制好：pod 'Charts', '~> 3.0.0' ，并且记住`Subspecs`下面的资源，（因为如果只pod charts的话，会出现无法找到Realm错误，所以请看第4步）

4. 查找`realm`：在第3步的图中的终端上输入`q`退出vim

   ``` 
   $ pod search realm
   ```

   可能会等待几分钟，最终出现如下图：![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9zq4vyidmj30fr09facd.jpg)

   复制好：pod 'Realm', '~> 2.0.4'

5. 编辑`Podfile`：在第4步终端中，输入`q`退出Vim，然后输入

   ``` 
   $ vim podfile
   ```

   然后在终端按下`i`键，输入如下内容：

   ``` 
   platform :ios, '8.0'
   use_frameworks!
   target 'ChartsDemoForBlog' ##这里是项目中target名字
   pod 'Charts', '~> 3.0.0'
   pod 'Realm', '~> 2.0.4'
   ```

   终端显示如下图：![](http://ww2.sinaimg.cn/large/006y8mN6gw1f9zqj4uvdoj30fw0a4t9b.jpg)

6. 退出vim：在步骤5终端中键入`esc`，然后键入`：`，输入`wq`回车，退出Vim

7. 下载库：在终端中输入以下内容：

   ``` 
   $ pod install
   ```

   PS：

   * 如果install提示本地没有realm或者charts库，那么就用`pod update` 更新本地库
   * 如果出现sh build.sh cocoapods-setup 错误，那么就删除项目所在目录中的pods文件夹，重新install

8. install完毕之后，打开.xcworkplace，会提示是否转swift3.0的提示，按照提示点击`convert`如图![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9zr1wznclj30yr0l2q50.jpg)

   ![](http://ww1.sinaimg.cn/large/006y8mN6gw1f9zr2djh38j30zc0ldwh4.jpg)

   ![](http://7xjwbl.com1.z0.glb.clouddn.com/E16E062E-ECC3-4496-8690-0F84DDEC891C.png)

   ![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9zr49ze69j30zg0lfgnh.jpg)

   这里会提示N个错误，别鸡冻，等Convert完之后，直接command+R运行，你会发现不一样的效果（报错全部消失）。

9. 创建Swift文件，如下图：![](http://7xjwbl.com1.z0.glb.clouddn.com/9F82A4C2-9600-4EB9-A083-1640C48CECC0.png)

10. 创建Objective-C bridging header，如下图：![](http://ww2.sinaimg.cn/large/006tNc79gw1f9zye3d5d0j30kd0ea0ua.jpg)

11. 引入头文件，在ViewController中引入`Charts-Swift.h`

    ``` objective-c
    #import <Charts/Charts-Swift.h>
    ```

    或者

    ``` swift
    @import Charts;
    ```

    两种方式引入Charts均可以实现，

12. 写测试代码，在`ViewDidLoad`写入以下代码：

    ``` objective-c
    - (void)viewDidLoad {
        [super viewDidLoad];
        BubbleChartView *chartView = [[BubbleChartView alloc] initWithFrame:CGRectMake(10, 10, 300, 300)];
        chartView.backgroundColor = [UIColor colorWithRed:0.184 green:1.000 blue:0.738 alpha:1.000];
        [self.view addSubview:chartView];
        
        PieChartView *pieView = [[PieChartView alloc] initWithFrame:CGRectMake(10, 350, 300, 300)];
        pieView.backgroundColor = [UIColor cyanColor];
        [self.view addSubview:pieView];
        // Do any additional setup after loading the view, typically from a nib.
    }
    ```

13. 效果如下图：![](http://ww2.sinaimg.cn/large/006tNc79gw1f9zyo464krj30af0j5wex.jpg)

### 结束

到此结束；Over！