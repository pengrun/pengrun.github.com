title: 'Swift 2.0: Session 3 学习'
date: 2015-06-24 09:14:27
categories: [Mobile]
tags:
- Swift
- 翻译

---
##申明
我**不是**一个**专业翻译人员**，肯定会有**翻译理解有误**的地方，很多**不确定**的都把**原文**贴出来了，附上了**自己的理解**，所以以下内容有哪里翻译有误请大家指出。

翻译来源：iBook中搜索 Swift 下载 [The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)
原文地址：[http://www.mrpeng.me/2015/06/24/Swift-2-0-Session-3-%E5%AD%A6%E4%B9%A0/](http://www.mrpeng.me/2015/06/24/Swift-2-0-Session-3-%E5%AD%A6%E4%B9%A0/)

##回顾
昨天主要学习了`let常量`和`var变量`的命名语法规范和不同类型之间相互转换的问题。
今天将要学习`Control Flow控制流`和`Functions and Closures函数和闭包`

<!--more-->

##Control Flow（控制流）
使用`if`和`switch`作为条件语句，使用`for-in`,`for`,`while`,`repeat-while`作为循环语句时，大括号`{}`外的条件或者循环变量是可选的，`{}`里面的主体是必须存在。

	```
	let individualScores = [75, 43, 103, 87, 12]
	var teamScore = 0
	for score in individualScores{
		if score > 50{
			teamScore += 3
		} else {
			teamScore += 1
		}
	}
	print(teamScore)
	```
	
> 答案：11


###if	

在`if`语句中，条件必须作为一个Boolean值来呈现--如果代码如：`if score{...}`就会报错。
如果你一起使用`if`和`let`的话，这些值就会丢失。这些值就作为了可选。可选值、包含值、nil都表明一个值的缺失。在一个变量类型的后面写一个问号，这样就作为了这个变量为可选。

	```
	var optionalString: String? = "Hello" //这个变量变为了可选值
	print(optionalString == nil)
	```
> 答案：false 
> 原因：optionalString = "Hello"   != nil  所以print(optionalString == nil) 为false
	
	```
	var optionalName: String? = "John Appleseed"
	var greeting = "Hello!"
	if let name = optionalName {
		greeting = "Hello,\(name)"
	}
	print(greeting)
	```
> 答案：Hello,John Appleseed
> 原因：let name = optionalName 判定为ture，所以会进入if语句
> EXPERIMENT
> Change optionalName to nil. What greeting do you get? Add an else clause that sets a different if optionalName is nil.
	eg:
	```
	var optionalName1: String? = nil
	var greeting1 = "Hello!"
	if let name = optionalName1 {
    greeting1 = "Hello,\(name)"
    } else {
    	greeting1 = "Hello,Mr.Peng"
    }
    println(greeting1)
	```
> 答案：Hello,Mr.Peng
> 原因：因为optionalName1 为nil -> let name = optionalName1 为false，所以无法进入if语句


###Switch
Swiches支持任意类型的数据并且可以执行大数据的比较，意思就是说不限于整形和`tests For equality(平等测试？)`

	```
	let vegetable = "red repper"
	switch vegetable {
	case "celery":
		let vegetableComment = "Add some reisins and make ants on a log."
	case "cucumber","watercress":
		let vegetableComment = "That would make a good tea sandwich."
	case let x where x.hasSuffix("pepper"):
		let vegetableComment = "Is it a spicy \(x)?"
	default:
		let vegetableComment = "Everything tastes good in soup."
	}
	```
> EXPERIMENT
> 
> Try removing the default case. What error do you get? 
> 
	error:
		Switch must be exhaustive, consider adding a default clause
		我理解的意思是：Switch中case均表示的开，既然switch从字面来说是开关，既然有开，那么必须存在关，那么default就是默认关闭状态，就必须存在，不能remove。
**注意**

细心的同学已经发现了，在Objective-C中Swich case后面必须跟上`break`。那么在这里为什么没有加上`break`呢？
> 原文：After executing the code inside the switch case that matched, the program exits form the switch statement. Execution doesn’t continue to the next case, so there is no need to explicitly break out of the switch at the end of each case’s code.
> 
* 我的解释：因为Swift中的switch执行到case里面之后，程序就将跳出switch。如果执行不到case里面，就会默认进入到default里面，执行完default之后，同样也会跳出switch循环。所以没必要一定要加入`break`让其强制跳出。当然如果你非要加入`break`呢，程序不会报错，也是可以运行的。

###for-in
你可以使用`for-in`遍历dictionary的每一对key-value对应的值。Dictionaries是无序集合，所以他们的keys和values遍历出来是没有顺序的。
	
	```
	let interestingNumbers = [
		"Prime": [2, 3, 5, 7, 11 ,13],
		"Fibonacci": [1, 1, 2, 3, 5, 8],
		"Square": [1, 4, 9, 16, 25],
	]
	var largest = 0
	for (kind, numbers) in interestingNumbers {
		for number in numbers {
			if number > largest {
				largest = number
			}
		}
	}
	print(largest)
	```
> 答案：25
> 原因：你可以把它看做是取dictionary中最大的value，因为在`if`语句中，是判断当前的value是否大于`largest`，如果大于那么`largest`的值重新被定义。所以最终取出最大的值。

###While
使用`while`去循环遍历代码块直到条件改变为止。循环中的条件能够在最后被替换，确保在循环中能够至少执行一次。
	
	XCode 7 or later:
	```
	var n = 2
	while n < 100 {
		n = n * 2
	}
	print(n)
	
	var m = 2
	repeat {
		m = m * 2
	}while m < 100
	print(m)
	```
	
	XCode 6.3 or earlier:
	```
	var n = 2
	while n < 100 {
		n = n * 2
	}
	print(n)
	
	var m = 2
	do {
		m = m * 2
	}while m < 100
	print(m)
	```
> 答案：128，128
> 疑问：虽说最终结果是等价，而且我也测试了很多数据也是一样的，但是始终有一个疑问，就是说，第一个是先判断，后计算，第二个是先计算，后判断，总感觉这里面有问题，可是一直未找到最终问题所在，如果有人明白的，请告诉我，谢谢。

上面两个循环分别等价于下面两个`for-in`

	```
	var firstForLoop = 0
	for i in 0..< 4 {
		firstForLoop += i
	}
	print(firstForLoop)
	
	var secondForLoop = 0
	for var i = 0; i < 4; ++i {
		secondForLoop += i
	}
	print(secondForLoop)
	```
使用`..<`表示小于上限值，使用`...`表示包括上限值。

##Functions and Closures（函数和闭包）
###Functions
使用`func`申明一个函数。按照其名称带有`圆括号()`中的参数列表调用函数。使用->从函数的返回类型中分离出的参数名和类型。

	```
	func greet(name: String, day: String) -> String {
		return "Hello \(name), today is \(day)."
	}
	greet("Bob", "Tuesday")
	```
使用一个复合的元组--例如，从函数中返回多个值。这个元组中的元素将和名字还有数字有关。

	```
	func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
		var min = scores[0]
		var max = scores[0]
		var sum = 0
		
		for score in scroes {
			if score > max {
				max = score
			} else if scroe < min {
				min = scroe
			}
			
			sum += score
		}
		return (min, max, sum)
	}
	let statistics = calculateStatistics([5, 3, 100, 3, 9])
	print(statistics.sum)
	print(statistics.2)
	```
> 答案：120；120
> 原因：其实这个方法转换成Objective-C的话，应该是这样：
> 
	- (NSArray *)calculateStatistices:(NSArray *)scores
	{
		....
		min = ..
		max = ..
		sum = ..
		return @[min,max,sum];
	}
> 所以`statistics.sum`和`statistics.2`的指针都是指向的元组中的第三个（0开始，2结束），这个`let statistics = calculateStatistics([5, 3, 100, 3, 9])`常量，其实可以理解为一个传参后返回的一个数组。

函数也能够接受数目可变的参数，收集他们到一个数组。
	```
	func sumOf(numbers: Int...) -> Int {
		var sum = 0
		for number in numbers {
			sum += number
		}
		return sum
	}
	let sum = sumOf()
    let sum1 = sumOf(42, 597, 12)
	```
> 答案：sum：0；sum1：651（42 + 597 + 12）

> EXPERIMENT
> 
	eg:
		```
		func averageOf(numbers: Float...) -> Float {
            var average : Float = 0.0
            var averageSum: Float = 0.0
            for number:Float in numbers {
                averageSum += number
            }
            
            if numbers.count != 0 { //如果不判断是否为空，其实也能编译成功，只是从编程严谨的角度上来说，最好写上。
                average = Float(averageSum) / Float(numbers.count)
            }
            return average
        }
        let average = averageOf();
        let average1 = averageOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10.10)
        println(average,average1)
		```
> 答案：average：0.0；average1：5.51

可以`nested(嵌套)`函数，嵌套的函数可以通过在外面申明的函数进行传值。你可以用嵌套函数来管理你很长或者很复杂的代码。
	
	```
	func returnFiftenn() -> Int {
		var y = 10
		func add() {
			y += 5
		}
		add()
		return y
	}
	returnFifteen()
	
	```
> 答案：15

函数是一个`first-class type（一流类型？）`。意思就是说一个函数能够以另一个函数作为返回值。
	
	```
	func makeIncrementer() -> (Int -> Int) {
		func addOne(number: Int) -> Int {
			return 1+ number
		}
		return addOne
	}
	var increment = makeIncrementer()
	increment
	```
函数可以将另一个函数作为其中参数
	
	```
	func hasAnyMatches(list: [Int], condition: Int -> Bool) -> Bool {
		for item in list {
			if condition(item) {
				reutrn true
			}
		}
		return false
	}
	
	func lessThanTen(number: Int) -> Bool {
		return number < 10
	}
	
	var numbers = [20, 19, 7, 12]
	hasAnyMatches(numbers, condition: lessThanTen)
	```
###Closures
函数其实是闭包的特殊情况：代码块能够延迟调用。闭包中的代码可以在创建闭包所在的范围内，能够访问变量和函数之类的东西，甚至闭包关闭了也能在不同的范围中执行--下面有一个例子。你可以写一个没有名字的闭包，代码放在大括号（`{}`）里面。使用`in`来分离参数和返回的类型。

	```
	numbers.map({
		(number: Int) -> Int in
		let result = 3 * number
		return result
	})
	```
> 答案：[3，6，9，12，15，18，21，24，27]
> 

> EXPERIMENT
> Rewrite the closure to return zero for all odd numbers
> 
	eg:
	```
	let allNumber = [1,2,3,4,5,6,7,8,9,]
	let odd = numbers.map({
		(number: Int) -> Int in
		if number % 2 != 0 {
			return 0
		}
        return number
    })
    println(odd)
	```
> 答案：[0, 2, 0, 4, 0, 6, 0, 8, 0]

有很多种方法能把闭包写的很简洁。当一个闭包的类型已经知道的时候，比如代理回调，就可以不用去在乎参数的类型。`Single statement closures implicitly return the value of their only statement(单条语句的闭包不能确切的返回他们唯一的语句值?)`
	
	```
	let mapedNumbers = numbers.map({number in 3 * number})
	print(mapedNumbers)
	```
你可以按名称引用参数而不是按数目引用---这种方法在很短的闭包中尤其有用。一个闭包作为最后一个参数传递给函数时，会立即出现在小括号(`()`)之后。当一个闭包只有一个参数传递给函数时，你可以省去小括号。

	XCode 7 or later
	```
	var numbers = [1,2,3,4,5,6,7,8,9]
	let sortedNumbers = numbers.sort { $0 > $1 }
	print(sortedNumbers)
	```
	
	XCode 7 earlier
	```
	var numbers = [1,2,3,4,5,6,7,8,9]
	let sortedNumbers = sorted(numbers) { $0 > $1 }
	print(sortedNumbers)
	```
##收获
###类型强制转换
Int转换为String
	
	```
	let intVar : Int = 3
	let strVar : String = String(intVar)	//方法1
	//let strVar : String = \(int)			//方法2 
	```
String转换为Int

	```
	let strVar : String = "123"
	let intVar : Int? = strVar.toInt()
	```
Double转换为String

	```
	Double.bridgeToObjectiveC().stringValue 
	```
Double保留两位小数稍麻烦些，需要对Double进行扩展：
	
	```
	extension Double {
    func format(f: String) -> String {
        return NSString(format: "%\(f)f", self)
    	}
	}
	let myDouble = 1.234567
	println(myDouble.format(".2")
	```
String转换Double

	```
	let strVar : String = "3.14"
	var string = NSString(string: strVar)
	string.doubleValue
	```
##结尾
由于自己的英语水平有限，和时间问题，今天就写到这里。

——————————————**2015-06-24**—————————————
> * 结束时间：2015-06-24 17:50 
> * 完成内容：完成`Control Flow控制流`和`Functions and Closures函数和闭包`两个板块
> * 感受：一天接受两个内容貌似有点多，因为有大量的单词信息，还有语法信息，所以后面每天一个板块，让自己充分理解和消化。
> * 明天计划：完成`Objects and Classes`	