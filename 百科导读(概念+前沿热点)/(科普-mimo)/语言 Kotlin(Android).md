```kotlin
println("Hello world")
//行注释

/*块注释
 *
 *
 */
```



# 变量

- type
- name
- value

一旦声明后(不论显式/隐式)，类型就确定了不能改变

- int
- double
- string

```kotlin
var num = 1
var str = "mike"
val	//不可变的变量

<num>++
<num>--

//声明数据类型
var y: String = "Allen"
```





# 语句

## 条件

```kotlin
if
else if
else
```





## 循环

```kotlin
repeat(4) {
    
}


while() {
    
}


for (<var> in <vars>) {
    
}
//2..5表示range(2, 5+1)

```





# 基本数据类型

## 布尔值

```kotlin
true
false

/*
==
!=
> <
>= <= 
*/


//判断
var x = false
x == false	//true

//应用：变量命名
isEating = true
```

逻辑运算符：

- and &&
- or ||
- !<var>



## 字符串

转义字符\

```kotlin
val char = 'a'

val greeting = "hello!"
var greeting = "hello!"

val output = "${<var>} ..."


//字符串方法
.length
.trim()	//去掉空白符
```





# 数据结构

## 数组

```kotlin
val array = array0f(15, 33, 7)
val output = Arrays.toString(lotteryNumbers)
println(output)


//数组方法
<>.get(<index>)
<>.set(<index>, <value>)
<>.size
```







## 数组列表

```kotlin
val friends = arrayList0f("Sam", "Amy", "Rex")


val numbers = arrayList0f<Int>()
numbers.add(1)

//方法
<>.get(<index>)
<>.add()
<>.removeAt(<index>)
<>.clear()	//清空内容


```





# 函数/方法

```kotlin
fun helloWorld(){
    
}

//限定数据类型
fun printMessage(text: String){
    println(text)
}

//限定函数返回的数据类型
fun getTwoTimes(number: Int): Int{
    return 2 * number
}
```





# 类

- Property/Attribute
- Behavior/Method

object是class的实例

```kotlin
class Person {
    var name: String = ""
    var age: Int = 0
    
    fun greet() {
        println("Hi, I am a persin!")
    }
}

val james = Person()


//属性在开头就声明
class Person(var age: Int = 0){
    
}

val james = Person(30)



//constructor
class Person(var fName: String = "", var lName: String = "") {
    var fullName: String = ""
    
    init {
        fullName = fName + " " + lName
    }
}
val indy = Person("Indiana", "Jones")	//也可命名参数：val indy = Person(fName = "Indiana", lName = "Jones")
println(indy.fullName)


//this
class Person {
    var feeling = ""
    fun updateStatus(feeling: String){
        this.feeling = feeling
    }
}
```





# null安全

null：表示no value(不等于0)

nullable: 值是null的变量



特征：加上`?`，表示nullable

- 可以使用safe call

```kotlin
var number: Int? = 0

//不能调用null对象的Property/Attribute和Behavior/Method
//以下代码报错：
class Car(var make: String = "") {
    
}

var ferrari: Car? = null	
/*等价于：
var ferrari: Car?	//一开始是null对象，直到后面完成实例化
ferrari = Car()
*/
print(ferrari.make)


//safe call
class Car(var price: Int = 0) {
    fun honk() {
        println("Beep beep!")
    }
}
var toyota: Car? = null
toyota?.honk()	//不会被call，因为toyota是一个null对象；但是前面带上? 调用method时不会报错
toyota?.price	//直接返回null
```



```kotlin
//创建类的时候：Property就是nullable
class Account(var name: String?, var email: String) {
    
}
var acct0ne = Account(null, "sam@mail.com")
println("Name: ${acct0ne.name}")	//Name: null
```

