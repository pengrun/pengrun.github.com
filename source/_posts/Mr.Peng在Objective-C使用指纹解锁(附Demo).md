title: Mr.Peng在Objective-C使用指纹解锁(附Demo)
date: 2015-06-27 10:50:13
categories: [Moblie]
tags:
- Objective-C
- TouchID
- Demo

---
##简介
由于工作需求手势解锁，而现在iOS8很多应用指纹替代了手势，就顺势去了解了下指纹解锁，网上很多资料都是以Swift来介绍的。对于刚接触Swift的我来说是不是有点太快了？所以查了下`Touch ID`的API，决定以Objective-C来写一个Demo。
##LocalAuthentication
官方的例子是和Keychain结合来讲TouchID[https://developer.apple.com/library/ios/samplecode/KeychainTouchID/Introduction/Intro.html](https://developer.apple.com/library/ios/samplecode/KeychainTouchID/Introduction/Intro.html)。而真正在我们开发过程中，很多时候都是直接获取我们在系统中设置的TouchID来验证。

<!--more-->

###<LocalAuthentication/LAPublicDefines.h>
首先导入`LocalAuthentication.framework`，在VC中引入`<LocalAuthentication/LAPublicDefines.h>`。Cmd + 左键查看`<LocalAuthentication/LAPublicDefines.h>`文件，大篇幅中只有两个方法：

	```
	- (BOOL)canEvaluatePolicy:(LAPolicy)policy error:(NSError * __autoreleasing *)error;
	- (void)evaluatePolicy:(LAPolicy)policy
       localizedReason:(NSString *)localizedReason
                 reply:(void(^)(BOOL success, NSError *error))reply;
	```
1、 第一个方法：就仅仅是验证设备是否支持TouchID，因为网上很多资料说还验证`设置中的TouchID`是否打开，经我自己验证，即使设置中TouchID关闭了，只要你之前设置了指纹了的，都是默认获取的。简单的说：`只要你手机是5S及5S以上，设置中有一个指纹的存在，那么这个方法返回的都是YES`。

	```
	 BOOL isAviailable = [locationAuth canEvaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics error:&error];
	```
2、 第二个方法：验证你指纹的正确和错误了。

	```
	if (isAviailable) {
        NSLog(@"指纹识别可用");
        //2、获取指纹验证结果
        [locationAuth evaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics localizedReason:@"请验证已有指纹" reply:^(BOOL success, NSError *error) {
            if (success) {
                NSLog(@"指纹验证成功");
                //后续操作
            } else {
                //验证错误3次则锁定，错误5此输入密码
                NSLog(@"指纹验证失败:%@,code:%d",error.localizedDescription,error.code);
                
                switch (error.code) {
                    case -1:
                        NSLog(@"超过重试次数 Application retry limit exceeded");
                        break;
                    case -2:
                        NSLog(@"用户取消 Canceled by user");
                        break;
                    case -3:
                        NSLog(@"选择密码验证 Fallback authentication mechanism selected");
                        break;
                    case -4:
                        NSLog(@"kLAErrorSystemCancel");
                        break;
                    case -5:
                        NSLog(@"kLAErrorPasscodeNotSet");
                        break;
                    case -6:
                        NSLog(@"kLAErrorTouchIDNotAvailable");
                        break;
                    case -7:
                        NSLog(@"kLAErrorTouchIDNotEnrolled");
                        break;
                        
                    default:
                        break;
                }
            }
        }];
    } else {
        NSLog(@"无法开启指纹识别");
    }
	```
	
Demo下载地址：[https://github.com/pengrun/TouchIDDemoForObc](https://github.com/pengrun/TouchIDDemoForObc)
原文地址：[http://www.mrpeng.me/2015/06/27/Objective-C%E4%BD%BF%E7%94%A8%E6%8C%87%E7%BA%B9%E8%A7%A3%E9%94%81/](http://www.mrpeng.me/2015/06/27/Objective-C%E4%BD%BF%E7%94%A8%E6%8C%87%E7%BA%B9%E8%A7%A3%E9%94%81/)
