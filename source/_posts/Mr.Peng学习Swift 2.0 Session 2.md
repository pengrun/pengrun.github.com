title: 'Mr.Peng学习Swift 2.0 Session 2'
date: 2015-06-23 14:52:01
categories: [Mobile]
tags:
- Swift
- 翻译

---
##申明
我**不是**一个**专业翻译人员**，肯定会有**翻译理解有误**的地方，很多**不确定**的都把**原文**贴出来了，附上了**自己的理解**，所以以下内容有哪里翻译有误请大家指出。

翻译来源：iBook中搜索 Swift 下载 [The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)
原文地址：[http://www.mrpeng.me/2015/06/23/Swift-2-0-Session-2-%E5%AD%A6%E4%B9%A0/](http://www.mrpeng.me/2015/06/23/Swift-2-0-Session-2-%E5%AD%A6%E4%B9%A0/)

##Swift介绍 （A Swift Tour）
传统建议在新语言中的第一个程序应该在屏幕上输出“Hello world!”在Swift中，这个操作只需要一个行代码即可搞定

	```
    print("Hello, world!");
    ```
<!--more-->
如果你曾经写过C或者Objective—C的话，上面那句语法是不是很熟悉呀----在Swift中，这一行代码就是一个完整的程序。你不需要为了一个功能去import单独的library库，比如input/output或者处理字符串。写在全局范围内的代码被用作程序的入口点，所以，你不需要一个 `main()`方法。你也不需要在每行代码后面添加分号。

这个介绍通过给你展示如何完成一个多样化的程序业务，来提供你足够多的资料去开始着手用Swift语言编码。如果在这篇介绍中你有不懂的或者全都不懂得，不用担心，在这本书的其余部分会有解释的。

> NOTE
> 
> On a Mac,download the Playround and double-click the file to open it in Xcode:[https://developer.apple.com/go/?id=prerelease-swift-tour](https://developer.apple.com/go/?id=prerelease-swift-tour)
	
##简单赋值（Simple Values）
使用`let`表示常量，`var`表示变量。`The value of a constant doesn't need to be known at compile time(常量不需要被编译)`但是你必须分配给它一个确切的值。意思就是说你可以使用使用常量来命名，如果你想在很多地方使用一个值，你就可以确定一个值来作为常量使用。
	
	```
	var myVariable = 4	2
	myVariable = 50
	let myConstant = 42
	```
常量或者变量都必须具有相同类型的值要分配给它。但是，你不必去写确切的类型。当你创建一个常量或者变量时，只需要提供一个值，让编译器去自动识别他的类型。在上面的例子中，编译器自动识别了`myVariable`是一个integer，因为它的初始化值是integer。

如果初始化的值不能提供足够多的信息（或者没有初始化），在可变的值后写明类型的话，要用冒号隔开。

	```
	let implicitInteger = 70
	let implicitDouble = 70.0
	let explicitDouble : Double = 70
	```
---
> EXPERIMENT
> 
> Create a constant with an explicit type of Float and value of 4.
> 
	eg:
		let explicitFloat : Float = 4.0
Values从不绝对的转换成另外的类型。如果你需要去转换一个值的类型，那就明确的替换成期望的类型。
	
	```
	let label = "The width is"
	let width = 94
	let widthLabel = label + String(width)
	```
	
---
> EXPERIMENT
> 
> Try removing the conversion to String from the last line.What error do you get?
> 
	Error:
		Binary operator '+' cannot be applied to operands of 		type 'String'and'Int'
	意思是：“+”不能实现，因为String Int类型不统一
	
有一个很简单的方式在strings中包含values：在`小括号()`里面写上这个value，并且在小括号前面写一个反斜杠`\`。如下：
	
	```
	let apples = 3
	let oranges 5
	let appleSummary = "I have \(apples) apples."
	let fruitSummary = "I hava \(apples + oranges) pieces of fruit."
    ```
	
------

> EXPERIMENT

> Use`\()`to include a floating-point calculation in a string and to include someone's name in a greeting.
> 
	eg：
		```
		let MrPengMoney = 99.2
		let MsZhouMoney = 100.2
		let MrPeng = "I hava \(MrPengMoney) RMB."
		let MsZhou = "I am more than MrPeng \(MsZhouMoney - MrPengMoney) RMB."
        ```
		
        
创建 arrays 和 dictionaries 使用方括号`[]`，通过写入方括号的index 或者 key来添加他们的元素。最后一个元素后面是允许写上一个逗号的`,`

	```
	var shoppingList = ["catfish","water","tulips","blue paint"]
	shoppingList[1] = "bottle of water"
	
	var occupations = [
		"Malcolm" : "Captain",
		"Kaylee" : "Mechanic",
		]
	occupations["Jayne"] = "Public Relations"
	```
    	
	
创建一个空得array 或者 dictionary，使用初始化语法
	let emptyArray = \[String]()
	let emptyDictionary = \[String : Float]()
如果能够推断出类型信息时，当你设置一个新的值作为变量或者传入一个参数到一个方法中，你可以`[]`作为空得array，`[:]`作为空得dictionary。例如
	
	```
	shoppingList = []
	occupations = [:]
    ```
        
	
##结尾
由于自己的英语水平有限，和时间问题，今天就写到这里。

——————————————**2015-06-23**—————————————
> * 结束时间：2015-06-23 17:40 
> * 完成内容：完成`Swift介绍`和`简单赋值`两个板块
> * 感受：初步感觉swift语法很干净简洁。也希望自己能够坚持翻译和写学习记录。
> * 明天计划：完成`Control Flow`和`Functions and Closures`
