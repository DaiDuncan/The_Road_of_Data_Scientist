应用：

- Duolingo
- Instagram

```swift
print("Hello, World!")

//单行注释
/*多行注释

*/

// 代码文件结构标记，可以显示文件的大致结构和模块
// MARK: - Properties
// MARK: - Initialization
// MARK: - IBAction Method
// MARK: - XXXDelegate


// 待完成标记， 代表此处有需要完成的功能或者后续开发
// TODO: - 待完成的功能
// TODO: - Need to finish
```



# 变量

## regular

```swift
var day = "Friday"
print(day)

var pi = 3.141
var number = 10.0 / 3

//限制类型
var user : String
user = "Alan"

//常数
let birthear = 1992
```



## optional

optional binding可以使用常数，也可以使用变量

比force-unwrapping更加安全：因为在使用之前，需要检测是否有值

```swift
var regularVariable: String
var optional: Int?

print(optional)	//nil


//
let optional: Int? = 10
if let value = optional {
    print(value, terminator: " ")	//结果是10
}
if optional != nil {
    print(optional!)	//!运算符不安全：需要提前确认是nil类型；结果是10
}



//
let name: String?
if var j = name {	//因为name没有赋值，所以if条件不成立
    print("James", terminator: " ")
}
var b = name ?? "Bond"	//"Bond"被??设置为默认值
print(b)
```



```swift
//!表示：隐式解包的可选implicity unwrapped optional
let value: Int!
value = 10
print(value)	//some(10) 如果没有复制，就会对程序造成破坏


let s = "10"
let number = Int(s)
print(number)	//Optional(10)：尝试从string value中获取number,就会得到optional
```





```swift
var b: Bird? = Bird()
b?.name = "Swifty"
if b?.makeSound() != nil {	//检测有.makeSound()方法
    if let n = b!.name {	//检测有.name属性
        print(n)
    }
}
```





# 语句

## 条件

```swift
if true/false {
    
}else if true/false {
    
}else {
    
}
```



## 循环

```swift
while ture/false {
    
}

for number in 1...3 {	//闭区间
    print(number)
}
```







# 基本数据类型

## 布尔值

```swift
var loggedIn = true
false

/*
==
!=
> <
>= <=
*/
```

逻辑运算符：

- and &&
- or ||
- !<var>



## 字符串

```swift
//字符串中的变量
var user = "Kate"
prin("user is: \(user)")

var number = 10
var word = String(number)

//方法
.isEmpty	//空字符返回true
.count
```





# 数据结构

## 数组

```swift
var friends = [String]()
friends.append("Harry")

var numbers = [1,2,3,4]

//方法
.count
```



## 字典

无序：也就无所谓第几个元素

```swift
var myDict: [String: String]	//声明
myDict = [String: String]()		//初始化

//直接声明+初始化
var fleet = ["Admiral": "Ackbar"]

//方法
.count
.isEmpty
.removeValue(forKey: <key>)
.updateValue(forKey: <key>)
.removeValueForkey(<key>)
```







# 函数/方法

```swift
func greeting() {
    print("Good morning")
}
greeting()


//限制返回的数据类型
func add() -> Int {
    
}

//限制参数数据类型
func add(age0ne: Int) -> Int {
   
}


//命名参数
func printAge(age: Int) {
    print(age)
}
var mac = 20
printAge(age: mac)


//默认参数: 注意下划线与变量名之间有空格
func greet(_ person: String) -> String {
    var time = "Good morning"
    var greeting = "\(time) \(person)"
    return greeting
}
var goodMorning = greet("Cheryl")
print(goodMorning)
```





# 类

blueprint：蓝图

- Property/Attribute
- Behavior/Method

```swift
class Human { 
    var name = ""
    var age = 0
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
var me = Human(name: "Joe", age: 25)



class Car {
    var color: String?	//带上?，可以不用init
    var mileage: Int
    init(){
        self.mileage = 0
    }
}
var myCar = Car()
print(myCar.color)	//结果是nil：没有值
```





## 继承

```swift
class Car {
    var color = "green"
}
class Racecar: Car {
    var nitro: Bool?
}
var myCar = Racecar()
print(myCar.color)
```



### 重写override

```swift
class Animal {
    func makeSound() {
        print("Arf!")
    }
}

class Dog: Animal {
    override func makeSound() {
        super.makeSound()	//"Arf!"
        print("Woof!")		//"Woof!"
    }
}
var myDog = Dog()
myDog.makeSound()
```



### 超类super

```swift
class Animal {
    var airborne = false
}
class Bird: Animal {
    override init(){
        super.init()		//在override init的情况下：必须要对超类先进行init => 超类实例化之后，才能调用超类的属性和方法
        self.airborne = true
    }
}
```







# 结构体struct

类似class：当我们只需要一些简单的变量集中在一起时，就可以用结构体替代类

- 如果需要继承的话，就要用class

@C语言：结构体



结构体有一个默认的initializer，所以initializer是可选的

```swift
struct Resolution {
    var width = 0, height = 0
}
let vga = Resolution()


//
struct Resolution {
    var width, height: Int
    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }
}
let vga = Resolution(width: 640, height: 480)
print(vga)


//
struct Position {
    var x = 0.0, y = 0.0
    // Return string value of coordinates
    var description: String {	//注意此处的定义：这是结构体的函数写法函数@结构体，而不是类
        return "[\(x), \(y)]"
    }
}
var position = Position()
print(position.description)
```

| 对象         |                    |
| ------------ | ------------------ |
| 更适合struct | class Point        |
|              | class Rectangle    |
| 更适合class  | class Human        |
|              | class Car: Vehicle |

```swift
struct Rectangle {
    var size = (5.0, 2.5)
    func area() -> Double {
        return size.0 * size.1
    }
}
```





# 枚举enum

并不是类

表示一系列有限的常数值(基于case来表示)

```swift
enum MyEnum{
    
}


enum Level {
    case beginner
    case intermediate
    case advanced
}
var myLevel = Level.beginner
myLevel = .advanced	//等价于Level.advanced

let myLevel = Level.beginner
print(myLevel)
```



```swift
//定义函数
enum Season {
    case spring, summer, fall, winter
    func description() -> String {
        return "a time of the year"
    }
}
var season = Season.fall
var season.2 = Season.winter
print(season.description())		//<string>
print(season2.description())	//<string>
```



```swift
//switch
var x = 2
switch x{
    case 0:
    	print("x=0")
    case 1：
    	print("x=1")
    default:
    	print("x is neither 0 nor 1")
}


//enum结合switch
enum Season {
    case spring, summer, fall, winter
    func description() -> String {
        switch self {
            case .summer:
            	return "hot season"
            case .winter:
            	return "cold season"
            default:
            	return "a time of the year"
        }
    }
}
var season = Season.winter	//初始化

//或者
var season: Season
season = .winter
```



## rawValue

值和实际意义相对应：

- 一周weekday
- 硬币1-25

对应有Int, Double, Float, String, Character

```swift
enum Grade: Double {
    case a, b, c, d, e, f
}
var grade = Grade.b
print(grade.rawValue)	//结果是1.0


//不同的数据类型不同
enum Product {
    case hardware(String)
    case software(String)
}
var myProduct: Product
myProduct = .hardware("Apple")
```



## 应用|判断情况

```swift
enum Product {
    case hardware(String)
    case software(String)
    var description: String {
        switch self {
            case .hardware(let name):
            	return "HW: \(name)"
            case .software(let name):
            	return "SW: \(name)"
        }
    }
}
var myPruduct: Product
myProduct = .hardware("Apple")

print(myPruduct.description)	//HW: Apple
```



示例|错误代码

- +需要明确类型
- cases可以有自定义的raw values，不需要写在一行

```swift
enum Level {
    case beginner = 1
    case intermediate = 2
    case advanced = 3
}
```





# 闭包closures

- 能够监测参数和返回值的类型
- 是函数的一种



使用场景：

- 需要另一个函数作为参数
- 语法简单、可读性强
- 能够和map()函数相结合

```swift
//简单函数
func square(numbder: Int) -> Int {
    return number * number
}
let myNumber = 3
print(square(number: myNumber))


//函数作为对象
let funcVar = square
print(funcVar(2))
```



## 函数作为参数

```swift
func myFunc(f: (Int) -> Int) {	//(Int) -> Int表示函数f的参数是Int，然后返回Int
    
}
func square(number: Int) -> Int {
    return number * number
}
let funcVar= square
myFunc(f: funcVar)	//square函数做为参数：也可以直接写myFunc(f: square)
```



## closures表达式

```swift
let a: (Int) -> Int
let b: (Int) -> Int
func square(number: Int) -> Int {
    return number * number
}
a = square
b = {(n: Int) -> Int in return n * n}	//本质上就是square函数，不过是用类似python列表生成式的方式


//简化1：不需要定义closures表达式中参数和返回的数据类型
b = {n in return n * n}	//更像python列表生成式
print(b(5))	//结果是25

//简化2：只要提前声明了，连return也可以省略
let a: (Int) -> Int
a = {n in n * n}
print(a(9))

//变式1：传递参数$<index>
let a: (Int) -> Int
a = {$0 * $0}
print(a(9))

let b: (Int, Int) -> Int
b = {$0 * $1}
print(a(3, 9))	//结果是27
```



- 结合map()

```swift
let myArray = [1,2,3]
let a: (Int) -> Int
a = {$0 * 2}
print(myArray.map(a))	//数组中每一个元素都进行函数a的运算：乘以2


//简化1
let myArray = [1,2,3]
print(myArray.map({$0 * 2}))
```



以下三者均等价

```swift
let a: (Int) -> Int
a = {$0 * $0}


let a: (Int) -> Int
a = {x in x * x}


func a(x: Int) -> Int {
    return x * x
}
```



```swift
print({$0 * $1 + $2}(1, 2, 3))	//结果是5
```

