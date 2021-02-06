- 强数据类型



```java
String greeting = "hello";
System.out.print(greeting + " world");

int myNumber = 4;
double myDouble = 4.0;

myNumber++

public class SpaceAdventure {
    public static void main
        (String[] args){
        
    }
}
```

## 开发平台

Eclipse

NetBeans



# 变量

```java
String myName = null;
```





# 语句

## 条件

```java
if () {
    
}else if () {
    
}else {
    
}
```





## 循环

do-while循环至少执行一次

```java
while () {
    
}


do {
    
}while();


for(int i= 1; i <=4; i++) {
    
}


String[] friends = {"Harry", "Ron", "Hermione"};
for (String f: friends) {
    System.out.print(f + ";");	
}
//output: Harry; Ron; Hermione;


for (String k: <HashMap>.keySet()) {
    
}
```







# 基本数据类型

声明(数据类型) + 初始化 + 赋值



primitive type原始类型：

- 布尔值

## 布尔值

```java
boolean truth;
truth = true;

/*
==
!=
> <
>= <=
*/

obja.equals(objb)
```

逻辑运算符：

- &&
- ||
- !<var>





## 字符串

`==`：检测是否为同一个object@python中的is

`<string>.equals(<string2>)`：检测值是否相同@python中的==

```java
String a = null;
if (a == null) {
    
}

char myChar = 'a';
String b = new String();

char[] mychars = {'B', 'o', 'b'};
String myString = new String(myChars);
System.out.print(myString);	//Bob
    
//常用方法
.length()
.isEmpty()
.concat()
.startWith();
.endwith();

```





# 数据结构

## 数组Array

单个数组只能存储一种类型的数据，需要声明和初始化

- 不能随时改变数组的大小

```java
String[] names;
names = new String[10];


String[] names = {"See", "how", "short", "this", "is"};
String output = Arrays.toString(names);	//以字符串的形式显示

int[] innocents = {};

String[] a;
a = new String[2];
a[0] = "Hey!";
System.out.print(a[0]);

//常用方法
<Array>.length
```





## 数组列表ArrayList

长度可变的Array

```java
ArrayList names;
names = new ArrayList();


//严格限制类型
ArrayList<String> names;
names = new ArrayList<>();
names.add("Jack");
names.add("John");
String s = String.join(",", names);	//Jack, John
System.out.print(s);

//.get()
String name = names.get(0);
//.set()
names.set(<index>, "John");
//.remove()
names.remove(<index>);

//其他方法
.size();	//.size()==0 等价于 .isEmpty()
```

Arrays.toString()将数组转换为可以输出打印的string

- 也可以用String.join(", ", myList)

ArrayList<String>.toArray()将字符串数组列表(可变的字符串数组)转换为数组(数组长度固定)



```java
//交换数组列表中的首、末元素
int lIndex = list.size() - 1;
String lElem = list.get(lIndex);
list.set(lIndex, list.get(0));
list.set(0, lElem);
```





## HashMaps

key-value对的无序集合：

- key和value可以是不同的数据类型
- 因为是键值对，所以初始化的时候是两个类型参数

类型不接受primitive type，比如int(String不属于primitive type => 可以使用Integer)

- Integer是一个对象，包含int-type的数据 => HashMap只接受object的类型参数
- <>表示类型引用type inference，初始化时，不用指明具体类型(当然，指明也没有问题，只不过显得多余)

```java
HashMap<String, String> map;	//声明
map = new HashMap<>();			//初始化

HashMap<Integer, String> map;
map = new HashMap<>();

//.put()也可以发挥update()的作用
map.put(23, "Michael");
//.get()
String value = map.get(23);	//key值
//,remove(<key>)

//其他方法
.size()
```





# 方法method

```java
//void表示没有返回值
void <func>(){	
    
}

void sayHi(String[] people) {
    String output = "";
    for (String p: people) {
        output += "Hi " + p + "! ";
    }
    System.out.print(output);
}

String[] myFriends = {"Ben", "Luke"};
sayHi(myFriends);


//*args
int totalize(int... numbers) {	//包括没有参数传入
    
}
```







# 类

constructor => object => instance of a class

object：

- property
- behavior

```java
class Dog {
    String name;
    void bark() {
        System.out.print("Woof!");
    }
    
    //Dog类的实例
    Dog(String name) {	
        this.name = name;
        bark();
    }
}

Dog myDog = new Dog("Baxter");



//继承：adapt
class Animal {
    int age = 0;
    void eat() {
        System.out.print("Munch!");
    }
}

class Dog extends Animal {
    String name;
    void bark() {
        System.out.print("Woof!");
    }
    
    //类似于初始化：constructor => 没有必要放在上述代码的后面：只是一种习惯
    //在constructor调用super, this
    //注意：必须以super()开头
    Dog(String name, int age) {
        super();	//继承Animal类
        this.name = name;
        this.age = age; 
        bark()
    }      
}


//重载overriding
class Dog extends Animal {
    ...;
    void eat() {
        ...;	//如果想要父类的方法：就用super.eat();
    }
    ...;
}
```







# main和modifiers⭐

Java中：

- 所有的代码都在class中
- 所有的程序都需要一个main()方法(在一个class中)
  - ==需要执行的程序==才需要main()方法
  - 参数是String array

## static

表示变量、方法是类的私有，而不属于某个具体的实例

比如：我们能够直接使用String.join()，而不需要创建String的实例 => 是因为它是static method

```java
class SpaceAdventure {
    public static void main
        (String[] args){
        ...;
    }
}

class MyApp {
    //main是函数的入口
    public static void main	
        (String[] args) {
        int myNumber = triple(2);	//不需要创建MyApp的实例，就可以使用这个方法
        System.out.print(myNumber);
    }
    //下述triple是一个static method，所以不需要创建MyApp的实例，就可以使用这个方法
    static int triple(int number) {
        return number * 3;
    }
}


//对比：非static方法
class MyApp {
    //main是函数的入口
    public static void main	
        (String[] args) {
        MyApp a;
        a = new MyApp();
        
        int myNumber = a.triple(2);	//需要创建MyApp的实例，才能使用这个方法
        System.out.print(myNumber);
    }
    //不是static method
    int triple(int number) {
        return number * 3;
    }
}
```



## private与public

public和private被称为modifiers

- 如果都没有：那么变量、方法就是package-private

```java
//script1.java
class Car {
    // Convert MPG to KM per Liter
    static double convert(double m) {
        return m * 0.425143707
    }
}


class Dog {
    private int hunger = 10;	//注意是private私有，不能访问
    void eat() {
        if (hunger > 0) {
            hunger--;
        }
    }
}

//main.java
class MyApp {
    publick static void main
        (String[] args) {
        double mpg = 26.9;
        double kpl = Car.convert(mpg);	//不需要创建实例(new Car).convert
        System.out.print(kpl);
    }
}
```



```java
class Message {
    private String message;
    
    public String readMessage
        (String reader) {
        if (reader.equals("Jar")) {	//如果访问者是"Jar"的话，就允许访问message
            System.out.print("Access granted");
            return message;
        }else {
            System.out.print("Access denied");
        }
    }
}
```

