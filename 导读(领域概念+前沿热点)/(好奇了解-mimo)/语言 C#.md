作用：游戏开发 => 从WOW到五子棋

```c#
Console.WriteLine ("Hello, World");
```



## me|印象

和JavaScript的语法很像



# 变量

```c#
var friend = "Sam";

var number = 10 / 3;
Console.WriteLine (number);
```







# 语句

## 条件

```c#
/*格式
if(){

} else if(){

}
...
else{}
*/
```





## 循环

```c#
i++;

var i = 1;
while (i <= 3){
    Console.WriteLine (i);
    i++;
}


for (var i = 1; i <= 3; i++) {
    Console.WriteLine (i);
}
```





# 基本数据类型

## 布尔值

```c#
true;
false;

/*
==
!=
> <
>= <=
*/


```

逻辑运算符：

- &&
- ||
- !<var>



## 字符串

```c#
string place = "New York";

char[] chars = {'a', 'b', 'c'};
string word = new String(chars);
Console.WriteLine(word);

//字符串方法
String.Concat(<str1>, <str2>);
<str>.Length;
<str1>.Contains(<str2>);
<str1>.Index0f(<str2>);	//返回int

<str>.Remove(<begin_index>, <num>);
<str>.Replace();
```





## 数据结构

## Arrays

- 一组内存
- 一种类型的数据
- 随时可以修改数据

```c#
//声明但不初始化
int[] numbers;
//声明一定空间
int[] numbers = new int[4];
//声明同时初始化
string[] names = {"a", "b", "c", "d"};


int[] ages = {30, 21, 27, 24};
var s = String.Join("-", ages);	//转变为字符串，方便输出显示
Console.WriteLine(s);

//其他方法
Array.Sort(<Array>);
<Array>.Length
```





## List 长度可变的Array

长度动态变化的数组(所以仍是一种类型的数据)

```c#
List<int> ages = new List<int> ();

var names = new List<string> ();
names.Add();

//从array中创建List
int[] array = {1, 2, 3};
var list = new List<int> (array);	//从array中创建List：这样list的长度可变
string s = String.Join(", ", list);	//打印时，记得转化为字符串
Console.WriteLine(s);	//结果是1，2，3

//List的其他方法
<List>.Count;
<List>.Insert(index, <element>);
<List>.Sort();
<List>.Remove(<element>);
<List>.RemoveAt(1)
<List>.Contains(<element>);
<List>.Index0f(<element>);
```







## Dictionary

key-value对

```c#
Dictionary<string, int> dict = new Dictionary<string, int> ();
var dict = new Dictionary<int, string> ()
{
    {23, "Michael"},
    {91, "Dennis"}
};


//字典方法
<>.Count;
<>.Keys;
<>.Values;
<>.Add();
<>.Clear();	//清空所有内容，但不会在内存中删除该数据
<>.Remove(<key>);
<>.ContainsKey(<key>);
<>.ContainsValue(<value>);
```







# 方法method

没有返回：void

```c#
static int Triple (int n) {
    int result = n * 3;
    return result;
}
var number = Triple (3);
Console.WriteLine(number);


static void SayHello (string name) {
    string str1 = "Hello ";
    string str2 = name;
    Console.WriteLine (str1 + str2);
}
SayHello ("Hodor");

//默认参数
static int Multiply (int i, int x = 3) {
    int result = i * x;
    return result;
}
int product = Multiply(10);
Console.WriteLine(product);

//*args
static int Totalize (params int[] numbers) {
    int result = 0;
    foreach (int number in numbers) {
        result += number;
    }
    return result;
}

Totalize(1);
Titalize(1,2,3);
```





# 类⭐

object：class的实例instance

- 用new方法来创建实例

```c#
class Car {
    string color;	//可以添加private或者指明public
    //constructor: 对应实例化的初始过程
    Car (string color) {
        this.color = color;
        StartEngine ();
    }
    void StartEngine () {
        Console.Write ("Vroom!");
    }
}
Car myCar = new Car ("red");


//不能访问private的property，那么就构造一个对应的public的property
class Car {
    private string color;
    //构造一个对应的public的property => 可以get和set
    public string Color {
        get {
            return color;
        }
        set {
            color = value;
        }
    }
}

Car vehicle = new Car ();
vehicle.Color = "Blue";
Console.Write (vehicle.Color);
```



```c#
//示例：创建constructor
class Dog : Animal {
    string name;
    //创建constructor
    Dog (string name) {
        this.name = name;
    }
}
```





```c#
//继承
class Vehicle {
    public int wheels = 4;
    public string brand = "Ford";
}

class Car : Vehicle {
    public string GetDetails () {
        return "This is a " + brand + " with " + wheels " wheels";	//继承了Vehicle的properties
    }
}

Car myCar = new Car ();
Console.Write (myCar.GetDetails());
```





# main与system⭐

## Main()

- 是程序的入口
- 需要在class中
- 可将命令行command-line作为参数



关于Main()的参数`string[] args`

- 名称总是args
- 当启动程序时，包含传递的实参

```c#
using System;

namespace App {
    class Program {
        public static void Main(string[] args)
        {
            SayHi();
        }
        static void SayHi(){
            Console.WriteLine("Hi!");
        }
    }
}
```



```c#
/* Car.cs */
using System;

class Car {
    private string color;	
    //constructor: 对应实例化的初始过程
    public Car (string color) {
        this.color = color;
    }
    public void StartEngine () {
        Console.Write ("Vroom!");
    }
}

/* Program.cs */
using System;

class Program {
    public static void Main(string[] args)
    {
        var c = new Car("red");
    }
}
```



## namespace

- 允许建立程序的良好结构
- 使用using关键字导入

```c#
//基于namespace: 可以区分相同名称的类 => namespace代号不同时
/* Car.cs */
using System;

namespace Classes {
    class Car {
        private string color;	
        //constructor: 对应实例化的初始过程
        public Car (string color) {
            this.color = color;
        }
        public void StartEngine () {
            Console.Write ("Vroom!");
    }
}
}


/* Program.cs */
using System;

namespace App {
    class Program {
    	public static void Main(string[] args) {
            var c = new Classes.Car("red");
        }
	}
}
```

因为`using System;`，所以就可以直接写`Console.Write()`，来代替`System.Console.Write()`



### 嵌套的namespace

```c#
using System.Collections.Generic;
namespace App {
    class Program {
        public static void Main(string[] args){
            var n = new System.Collections.Generic.List<int> ();
            var m = new List<int> (); //可以简化使用List
            n.Add(8);
            m.Add(1);
            m.Add(n[0]);
            string s = String.Join(", ", m);
            Console.Write(s);
        }
    }
}
```



### 同一个namespace

```c#
/* Main.cs */
using System;

namespace App{
    class Program {
        public static void Main(string[] args){
            var a = new Animal("Sparky", "dog");
            var s = sparky.Describe();
            Console.Write(s);
        }
    }
}

/* class.cs */
using System;

namespace App{
    class Animal{
        private string name;
        private string species;
        
        public Animal(string name, string species){
            this.name = name;
            this.species = species;
        }
        public string GetDetails(){
            var s = name + ", " + species;
            return s;
        }
    }
}
```





# #补充|static

如果不声明static，就要创建相应的实例

```c#
using System;

class Program {
    public static void Main(string[] args){
        SayHi();
    }
    static void SayHi(){
        Console.WriteLine("Hi!");
    }
}


//没有声明static时
using System;

class Program {
    public static void Main(string[] args){
        var a = new Program();
        a.SayHi();
    }
    void SayHi(){
        Console.WriteLine("Hi!");
    }
}
```

