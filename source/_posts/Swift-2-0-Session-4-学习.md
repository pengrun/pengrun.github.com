title: 'Swift 2.0 Session 4 学习'
date: 2015-06-25 09:33:43
categories: [Mobile]
tags:
- Swift
- 翻译

---

##申明
我**不是**一个**专业翻译人员**，肯定会有**翻译理解有误**的地方，很多**不确定**的都把**原文**贴出来了，附上了**自己的理解**，所以以下内容有哪里翻译有误请大家指出。

翻译来源：iBook中搜索 Swift 下载 [The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)
原文地址：[http://www.mrpeng.me/2015/06/25/Swift-2-0-Session-4-%E5%AD%A6%E4%B9%A0/](http://www.mrpeng.me/2015/06/25/Swift-2-0-Session-4-%E5%AD%A6%E4%B9%A0/)

##回顾
昨天主要学习了`Control Flow控制流`和`Functions and Closures函数和闭包`
今天将要学习`Objects and Classes 对象和类`

##Objects and Classes
使用`class`时要通过类名去创建一个类。类中属性的申明同常量或者变量申明方式一样，不同之处在于属性贯穿全类的。同样的，方法和函数的申明也是一样。
<!--more-->
	```
	class Shape {
		var numberOfSides = 0
		func simpleDescription () -> String {
			return "A shape with \(numberOfSides) sides."
		}
	}
	```

> **EXPERIMENT**
> Add a constant property with let,and add another method that takes an argument.
> 
	eg:```
		override func viewDidLoad() {
			super.viewDidLoad()
        	var age = Age()
        	var mrpengAge = age.getAge()
        	println(mrpengAge)
        	// Do any additional setup after loading the view, typically from a nib.
        	}
		class Age {
        	let mrpengAge = 27
        	func getAge () -> String {
            	return "Mr.Peng,\(mrpengAge) years old.Oh,so old!"
        		}
    		}
		```
新建一个实例类只需要在类名后面加入小括号(`()`)就可以了。调用这个实例类的属性或者方法在后面用(`.`)引出。

	```
	var shape = Shape()
	shape.numberOfSides = 7
	var shapeDescription = shape.simpleDescription()
	```
`This version of the shape class is missing something important:an initializer to set up the class when an instance is created(这个版本的Shape类缺少了一些重要东西：当实例被创建时就已经初始化的设置了?)`
	
	```
	class NamedShape {
		var numberOfSides: Int = 0
		var nameL: String
		
		init(name: String) {
			self.name = name
		}
		
		func simpleDescription() -> String {
			return "A shape with \(numberOfSides) sides."
		}
	}
	```
通过初始化，`self`被用作区分`name`属性和`name`参数。当你创建类的实例时，该参数初始化就像一个函数被调用一样。每个属性在申明中或者在初始化中都应该有值的分配。

如果在对象被释放之前你需要去清除实例的话，就使用`deinit`创建一个析构器[^1x]

子类(Subclasses)在类名后面跟上父类(Superclass)的名字，用冒号(`:`)分隔。有些类继承任何root class是没有要求，所以你可以在类名后面跟上父类或者省略父类都可以。

	```
	class Square: NamedShape {
		var sideLength: Double
		
		init (sideLength: Double, name: String) {
			self.sideLength = sideLength
			super.init(name: name)
			numberOfSides = 4
		}
		
		func area() -> Double {
			return sideLength * sideLength
		}
		
		override func simpleDescription() -> String {
			return "A square with sides of length \(sideLength)."
		}
	}
	
	let test = Square(sideLength: 5.2, name: "my test square")
	test.area()
	test.simpleDescription()
	```
> **EXPERIMENT**
> Make another subclss of `NamedShape` called `Circle` that takes a radius and a name as arguments to its initializer. Implement an `area()` and a `simpleDescription()` method on the `Circle` class.
> 
	eg:```
		class Circle: NamedShape {
        	var radius : Double
        	let π: Double = 3.1415926
        	init (radius: Double, name: String) {
        	    self.radius = radius
        	    super.init(name: name)
        	}
        	func area() -> String {
        	    return "\(self.name) with area is \(π * radius * radius)"
       		 }
        	override func simpleDescription() -> String {
        	    return "\(self.name) with radius \(radius)"
        	}
    	}
    	let circle = Circle(radius: 2.5, name: "Mr.Peng's Circle")
        println(circle.area(),circle.simpleDescription())
		```
除了简单的属性存储外，属性还有`getter`和`setter`

	```
	class EquilateralTriangle: NamedShape {
		var sideLength: Double = 0.0
		
		init (sideLeght: Double, name: String) {
			self.sideLength = sideLength
			super.init(name: name)
			numberOfSides = 3
		}
		
		var perimeter: Double {
			get {
				return 3.0 * sideLength
			}
			
			set {
				sideLength = newValue / 3.0
			}
		}
		
		override func simpleDescription() -> String {
			return "An equilateral triangle with sides of length\(sideLength)."
		}
	}
	
	var triangle = EquilateralTriangle(sideLength: 3.1, name:"a triangle")
	print(triangle.perimerter)
	triangle.perimerter = 9.9
	print(triangle.sideLength)
	```
在`perimeter`setter中，新值有不确切的名字`newValue`，你可以在`set`后面加个冒号(`:`)然后写上一个确切的名字。

这个`EquilateralTriangle`构造器重有三个不同的步骤：
	1.设置申明子类属性的value
	2.调用父类的构造器
	3.通过定义父类改变属性value.一些额外的工作：methods、getters、setters均能在这个里处理。
如果你不需要去计算属性，但运行前、设置新值之后仍要提供代码，那么久使用`willSet`和`didSet`。

	```
	class TriangleAndSquare {
		var triangle: EquilateralTriangle {
			willSet {
				square.sideLength = newValue.sideLength
			}
		}
		
		var square: Square {
			willSet {
				triangle.sideLength = newValue.sideLength
			}
		}
		
		init(size: Double, name: String) {
			square = Square(sideLength: size, name: name)
			triangle = EquilateralTriangle(sideLength: size, name: name)
		}
	}
	
	var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
	print(triangleAndSquare.square.sideLength)
	print(triangleAndSqueare.triangle.sideLength)
	triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
	print(triangleAndSquare.triangle.sideLength)
	```
> 答案：10，50，50
> 原因：看懂代码就很简单了。

当需要加入可选值时，你应该写上`?`在像操作methods，properties，subscriptin之前。如果在`?`前的值是一个`nil`，在`?`后面的任何东西都可以忽略，并且所有的值都可以表示为`nil`。相反，当可选值是`unwrapped（解开？）`，在`?`后面的东西都可以表示为unwrapped value。这两种情况，都可以表示为可选值。

	```
	let optionalSquare: Square? = Square(sideLength: 2.5, name:"optional square")
	let sideLength = optionalSquare?.sideLength
	```
##结尾
由于自己的英语水平有限，和时间问题，今天就写到这里。

——————————————**2015-06-25**—————————————
> * 结束时间：2015-06-25 16:50 
> * 完成内容：完成`Objects and Classes 对象和类`一个板块
> * 感受：后面内容越来越复杂了，不能为了写博客而写博客，自己还是要通过操作来消化理解。
> * 明天计划：完成`Enumerations and Structures枚举和结构体`

[^1x]:析构器(Deinitializer)与构造器(Initializer)相反，在对象释放时候调用,使用关键字 deinit。

