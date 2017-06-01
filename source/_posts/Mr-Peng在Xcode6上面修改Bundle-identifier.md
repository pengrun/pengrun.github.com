title: Mr.Peng在Xcode6修改Bundle identifier(有pod)
date: 2015-08-20 11:57:13
categories: [Moblie]
tags:
- 修改项目名
- Bundle identifier
- Xcode6

---
##简介
肯定很多人在公司开发项目时，可能项目的名字还未定下来，但是我们不可能等项目名定下来之后再动手，就只有先起一个临时的`Bundler identifier`，可能有些人到后面也不会去太在意这个`Bundler identifier`，但是对于我来说，我非要把`Bunder identifier`给改回来，现在我介绍下在有`pod`的情况下如何修改`Bundler identifier`，如果在项目中没有用到`pod`的，请移步[http://m.blog.csdn.net/blog/tanhailong198801/40148525](http://m.blog.csdn.net/blog/tanhailong198801/40148525)。

本人原文地址：原文地址：[http://www.mrpeng.me/2015/08/20/Mr-Peng在Xcode6上面修改Bundle-identifier](http://www.mrpeng.me/2015/08/20/Mr-Peng在Xcode6上面修改Bundle-identifier)

<!--more-->

##步骤
####备份
这必须作为第一步去处理的，先手动备份一份原项目。
####删除.xcworkspace、.lock文件
如果项目中用到了`pod`的，先打开项目所在目录，如我的项目名：`RoadWizard`,把`RoadWizard/Podfile.lock`、`RoadWizard/RoadWizard.xcworkspace`、`RoadWizard/Pods/Manifest.lock`三个文件删掉。如下图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508203.png)
####打开.xcodeproj修改名字
1.	在左侧导航栏区域点击两次项目名，如图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508201.png)
2.	修改完成之后，回车，会出现以下情况，直接`Rename`，如图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508202.png)
3.	`Rename`之后会提示`Enable`，然后直接`Enable`，如图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508204.png)
4.	在导航区域选中`RoadWizard`，右键点击`Show in Finder`，在弹出的Finder中修改`RoadWizard`为`CarWaiter`，如图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508205.png)
5.	修改了Finder中的名字后，你会发现项目中的所有文件全变红色了，表急，还是在导航区选中`RoadWizard`，然后在右侧把`RoadWizard`修改为`CarWaiter`，然后在下面有一个`文件按钮`，点击之后选中`第四步`修改的文件。如下两图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508206.png)![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508207.png)
6.	用第5步的方法同样修改`RoadWizardTests`文件，修改完成之后，打开`RoadWizardTests.m`文件，然后在右边先把`RoadWizardTests.m`改为`CarWaiterTests.m`，然后在`.m`文件中右键名字，点击`Rename`修改为`CarWaiterTests`,如下图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508208.png)
####修改Scheme
如图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015082010.png)点击`Manage Scheme`，然后连续点击`RoadWizard`名字（有点不好点），然后变成可编辑状态，输入`CarWaiter`,然后回车，点击`close`，如下图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/20150820111.png)
####修改Test的Targers
点击项目，选中`CarWaiterTest`，然后点击`General`，把`Host Application`这一栏选中`CarWaiter`,如下图：![image](http://7xjwbl.com1.z0.glb.clouddn.com/2015082012.png)
####修改.pch路径
点击项目，选中`Targets`，点击`Build Settings`，在搜索栏输入`pch`,定位到如下位置：![image](http://7xjwbl.com1.z0.glb.clouddn.com/201508209.png)，然后把`RoadWizard`修改为`CarWaiter`
####update Pod
如果有`Pod`的，这时候`update Pod`，然后会再项目文件夹下面重新出现之前删除的`.xcworkspace`、`.lock`这两个文件。

##结束
到这里，修改`Bundler identifier`就算高一段落了。希望对大家有所帮助。如果哪里有问题的，请告诉我，我好修改，谢谢。