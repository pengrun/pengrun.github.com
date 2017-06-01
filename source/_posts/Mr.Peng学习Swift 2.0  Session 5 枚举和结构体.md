title: Mr.Peng学习Swift 2.0 Session 5  枚举和结构体 
date: 2015-06-26 14:38:00
categories: [Mobile]
tags:
- Swift
- 翻译

---
##申明
我**不是**一个**专业翻译人员**，肯定会有**翻译理解有误**的地方，很多**不确定**的都把**原文**贴出来了，附上了**自己的理解**，所以以下内容有哪里翻译有误请大家指出。

翻译来源：iBook中搜索 Swift 下载 [The Swift Programming Language (Swift 2 Prerelease)](https://itunes.apple.com/cn/book/swift-programming-language/id1002622538?mt=11)
原文地址：[http://www.mrpeng.me/2015/06/26/Swift-2-0-Session-5-%E6%9E%9A%E4%B8%BE%E5%92%8C%E7%BB%93%E6%9E%84%E4%BD%93/](http://www.mrpeng.me/2015/06/26/Swift-2-0-Session-5-%E6%9E%9A%E4%B8%BE%E5%92%8C%E7%BB%93%E6%9E%84%E4%BD%93/)

##回顾
昨天主要学习了`Objects and Classes 对象和类`
今天将要学习`Enumerations and Structures 枚举和结构体`

###Enumerations 枚举
<!--more-->
使用`enum`来创建结构体。像类和其他所有命名类型一样，枚举可以包含methods。
​	
	​```
	enum Rank: Int {
		case Ace = 1
		case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
		case Jack, Queen, King
		func simpleDescription() -> String {
			switch self {
				case .Ace:
					return "ace"
				case .Jack:
					return "jack"
				case .Queen:
					return "queen"
				case .King:
					return "king"
				default:
					return String(self.rawValue)
			}
		}
	}
	let ace = Rank.Ace
	let aceRawValue = ace.rawValue
	​```
> **EXPERIMENT**
> Write a function that compares two `Rank` values by comparing their raw values.
> ​	
	eg:```
	func compare(a: Rank, b: Rank) -> String {
	var bigger: Rank = a
	if b.rawValue > a.rawValue {
		bigger = b
		}
		return "The bigger of the two is \(bigger.simpleDescription())"
	}
	let two = Rank.Two
	let ten = Rank.Ten
	let result = compare(two, ten)
	println(result)
	​```
在上面的例子中，枚举中的raw-value类型是`Int`，所以你只需要设置第一个原始值。剩下的原始值会按照顺序赋值。你也可以使用字符串或者浮点型作为枚举的原始值。使用`rawValue`属性访问枚举成员的原始值。

使用`init?(rawValue:)`初始化一个从原始值获取到的枚举实例。

	​```
	if let convertedRank = Rank(rawValue: 3) {
		let threeDescription = convertedRank.simpleDescription()
	}
	​```
枚举的成员值是实际值，并不是原始值的另一种写法。实际上，如果原始值没有意义，你也不需要去设置它。

	​```
	enum suit {
		case Spades, Hearts, Diamonds, Clubs
		func simpleDescription() -> String {
			switch self {
				case .Spades:
					return "spades"
				case .Hearts:
					return "hearts"
				case .Diamonds:
					return "diamonds"
				case .Clubs:
					return "clubs"
			}
		}
	}
	let hearts = Suit.Hearts
	let heartsDescription = hearts.simpleDescription()
	​```
> **EXPERIMENT**
> Add a color() method to Suit that returns "black" for spades and clubs,and returns "red" for hearts and diamonds.
>
	eg:```
	func color() -> String { 
	    switch self { 
	    case .Spades, .Clubs: 
	        return "black" 
	    case .Hearts, .Diamonds: 
	        return "red" 
	    } 
	}
	​```
注意，上面引用了枚举的`Hearts`成员的两种方式：给`hearts`赋值常量时，枚举成员`Suit.Hearts`通过它的全名来引用，因为常量没有显示指定的类型。在switch里面，枚举成员通过`.Hearts`缩写来引用，因为`self`的值已经知道是一个suit。当值的类型确定时，你可以使用缩写格式。

###结构体
使用`struct`来创建一个结构体。结构体和类很多地方相同。比如方法和构造器。**最大的不同一点：`结构体`是值类型，而`类`是引用类型即：分别创建一个结构体和一个类，然后分别初始化两个结构体(struct1，struct2)和两个类(class1,class2)实例，再分别给这一个结构和一个类赋值，那么另外一个结构体的值没有改变，而另外一个类的值改变了**具体请传送[The Swift Programming Language--语言指南--类和结构体](http://www.cocoachina.com/ios/20140612/8780.html)。

	​```
	struct Card {
		var rank: Rank
		var suit: Suit
		func simpleDescription() -> String {
			return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
	        }
	    }
	    let threeOfSpades = Card(rank: .Three, suit: .Spades)
	    let ThreeOfSpadesDescription = threeOfSpades.simpleDescription()
	​```
> EXPERIMENT
> Add a method Card that creates a full deck of cards,with one card of each combination of rank and suit.
>
	eg：```
	class Card {
		var rank: Rank
		var suit: Suit
		func simpleDescription() -> String {
			return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
		}
		init(cardRank: Rank, cardSuit: Suit) {
			self.rank = cardRank
			self.suit = cardSuit
		}
		func Deck() -> String {
			var cardName = ""
			for i in 0...14 {
				if let convertedRank = Rank(rawValue: i) {
					self.rank = convertedRank
					for y in 0...5 {
						//一定要把Suit的枚举定义成Int类型,不然这里会报错的
						if let convertedSuit = Suit(rawValue: y) {
							self.suit = convertedSuit
							cardName = "\(cardName) \(self.simpleDescription())"
						}
					}
				}
			}
			return cardName
	    }
	} 
	    let threeOfSpades = Card(cardRank: .Three, cardSuit: .Spades) 
	    let threeOfSpadesDescription = threeOfSpades.simpleDescription() 
	    threeOfSpades.Deck()
	​```
一个枚举成员的实例可以有实例值。同样的枚举成员的实例可以有不同的实例值。你可以在创建实例时传入值。传入的值和原始值是不同的：枚举成员的原始值对于所有实例来说都是相同的，并且是你在定义枚举时设置原始值。

例如，考虑从服务器请求日出和日落时间。服务器会返回正常结果或者错误信息。

	​```
		enum ServerResponse {
			case Result(String, String)
			case Error(String)
		}
		let success = ServerResponse.Result("6:00 am", "8:09 pm")
		let failure = ServerResponse.Error("Out of cheese.")
		
		switch success {
			case let .Result(sunrise, sunset):
				let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
			case let .Error(error):
				let serverResponse = "Failure...\(error)"
		}
	​```
> **EXPERIMENT**
> Add a third case to `SearverResponse` and to the switch.
>
	eg:```
	enum ServerResponse {
		case Result(String, String)
	    case Error(String)
	    case ThirdCase(String)
	}
	let success = ServerResponse.Result("6:00 am", "8:09 pm")
	let failure = ServerResponse.Error("Out of cheese.")
	    
	switch success {
	    case let .Result(sunrise, sunset):
	        let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
	        println(serverResponse)
	    case let .Error(error):
	        let serverResponse = "Failure...\(error)"
	        println(serverResponse)
	    case let .ThirdCase(thirdCase):
	        let serverResponse = "ThirdCase is \(thirdCase)"
	        println(serverResponse)
	    }
	​```
——————————————**2015-06-26**—————————————
> * 结束时间：2015-06-26 17:50 
> * 完成内容：完成`Enumerations and Structures枚举和结构体`一个板块
> * 明天计划：完成`Protocol and Extensions代理和扩展`
