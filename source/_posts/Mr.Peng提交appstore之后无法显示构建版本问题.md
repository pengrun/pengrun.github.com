## Mr.Peng提交appstore之后无法显示构建版本问题

title: 'Mr.Peng提交appstore之后无法显示构建版本问题'

categories: [Moblie]
tags:

- XCode 8
- iOS 10
- appstore

------

最近通过XCode 8 提交appstore后，在构建版本里面一直无法显示上传的版本， 通过`Application Loader`、和`XCode`两种方式上传均未显示版本。经查阅资料，原来是iOS 10更加注意用户隐私数据，所以需要调用权限时，都应该在info.plist里面加上配置。例如需要定位(Location When In Use Usage Description)，就必须在`info.plist`加上`Privacy - Location When In Use Usage Description` 并且在后面写上对应的描述`XXX需要获取地理位置信息`，如下图：

![](http://ww4.sinaimg.cn/large/006y8mN6gw1f9n5bv7svqj30gk03et9l.jpg)

如 SDK 用到的相关功能：

| SDK名字 | 配置                                       | 描述               |
| ----- | ---------------------------------------- | ---------------- |
| 麦克风权限 | Privacy - Microphone Usage Description   | 是否允许XXX使用您的麦克风   |
| 相机权限  | Privacy - Camera Usage Description       | 是否允许XXX使用您的相机    |
| 相册权限  | Privacy - Photo Library Usage Description | XXX需要访问您的相册      |
| 定位权限  | Privacy - Location When In Use Usage Description | XXX需要获取地理位置信息    |
| 定位权限  | Privacy - Location Always Usage Description | XXX需要在后台获取地理位置信息 |

PS：

* 上面的中文提示您可以按照您的产品需求进行定制。
* 如果你项目里面没有用到任何需要权限的SDK，而在`itunesConnect`里面也一直无法显示上传的版本，不妨尝试加上这些配置试一试。