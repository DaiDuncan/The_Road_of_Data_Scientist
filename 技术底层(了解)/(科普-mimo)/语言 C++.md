```c++
std::string name = "Henry";
std::cout << "Hi there, " << name;


#include <iostream>

//执行的程序入口
int main()
{
    std::cout << "Hello World!";
    return 0;
}
```



开发平台：

InterlliSense

NetBeans



Build => compile => run/excute



```c++
#include <iostream>
using namespace std;	//std是标准库：使用命名空间类似于python中的from <> import <>

int main()
{
    string name = "Bob";
    cout << "Hi, " << name << endl;
}
```



# 变量

声明：

- type
- name

初始化

赋值

```c++
string name = "Jane";
string name("Jane");

int num(10);

//编译器自己决定数据类型
// 如果使用auto声明就一定要初始化，不能单独声明
auto name = "Jane";


string city, state;
city = state = "Salzburg";
double x = 47.8095, y = 13.0550;
cout << city << ": " << ", " << y;
```



```c++
int age, laps = 1;
cout << age << endl;	//结果是0：没有初始化的变量的值是随机的
cout << laps;


double x, y;
x = y = 48.2082;
y -= 31.8344;
cout << x << endl << y;	//结果是48，2082  16.3738：说明x=y=<num>的方式指向的是两块不同的内存地址
```





# 语句

## 条件

```c++
#include <cstdlib>
#include <iostream>
using namespace std;

int main()
{
    cout << "Print this";
    exit(0);	//跳出程序 0表示成功的exit
    cout << "Don't print this";
}


if ()
{
    
}
else if ()
{
    
}
else
{
    
}


switch (num)
{
    case 1:
        ...;
        break;
    case 2:
        ...;
        break;
        ...;
    default:
        cout << "Invalid input";
        break;
}
```





## 循环

```c++
while()
{}

for(i=0; i < <var>; i++)
{}

do
{}while();


break;
continue;


int i = 0;
while(i <= 3)
{
    cout << i;
    i++;
}
```







# 基本数据类型

## 布尔值

```c++
true;
false;

std::boolalpha;//显示true或false, 否则显示1或0
cout << <BOOL>;
cout << boolalpha << <BOOL>;	//显示true或false
    
bool falsetto = false;

/*
==
!=
> <
>= <=
*/


//注意运算符优先级
cout << (12 >= 10); //如果不写括号，<<的优先级可能高于>=
```

逻辑运算符：

- &&
- ||
- !<var>



## 字符串

本质上就是一个数组array

```c++
/* 
\0是字符串的尾字符 null
\n是换行符

*/
char ourString[7] = {'W', 'a', 's', 's', 'u', 'p', '\0'};	//一般使用string方法，而不是这样繁琐的初始化
cout << ourString;	//Wassup

string str = "";

//字符串方法
.length();
.find(<子字符串>);	//返回首字符的index

.erase(<index_start>, <index_end>);	//删除index_start到index_end之间的所有字符(如果只有index_start，那么之后的都删除)
```





# 数据结构

## 数组Array

元素是序列，而且是同一种类型

```c++
string ourArray[10];

int ages[6] = {18, 19, 20, 21, 22, 23};

int ourArray[3] = {0, 100, 200};
ourArray[3] = 300;
cout << ourArray[3];	//输出是300：C++不会检查新的元素是否能够加入array

//多维数组
int keypad[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
cout << keypad[1][1];	//结果是5

//数组方法
sizeof();	//返回字节数
sizeof(<Array>) / sizeof(<Array element>);	//返回数组的元素个数

```





## 指针⭐

动态内存分配：在这个过程中，指针很有用

- stack：已经声明的变量
- heap：待分配的内存空间

```c++
/*
& 取地址运算符
* 对应&，从地址中取出对象的值
*/
string str = "Point here";
int num = 23;
cout << &str << " and " << &num;

int num = 250;
cout << &num << endl;
cout << *&num;

double speed = 10.5;
double *spdPtr = &speed;	//指针的类型与指针指向对象的类型一致

//null指针
//类型改为double string均可
int *ptr = 0;
int *nullPtr = NULL;
cout << ptr << " & " << nullPtr;	//结果是0 & 0：都是null指针，地址是0



//null指针，地址是0
int *nullPtr = NULL;
if (nullPtr == 0)	//null指针的判断
    cout << "That's a null pointer";
else
    cout << *&nullPtr
```



```c++
int ptrArr[3] = {0, 1, 2};
int *ptr;
ptr = ptrArr;

cout << *ptr;
ptr++;	//选取下一个数组元素
cout << *ptr;
ptr++;
cout << *ptr;
ptr++;

//数组名array与&array[0]的值是同一个地址 => 可以让指针名=数组名
```







# 函数function

声明时：

- 函数名
- 任何参数(可能没有参数)
- 返回类型
  - 没有指明返回类型，则默认为void

```c++
string joinStrings(string str1, string str2)
{
    string str3 = str1 + str2;
    return str3;
}
cout << joinStrings("Hello ", "World");

//default 参数
int multiplyBy(int i, int x=2)
{
    int y = i * x;
    return y;
}
cout << multiplyBy(3);


//传递指针
void changeValue(int *pointer)
{
    *pointer = 100;
}
int num = 0;
changeValue(&num);
cout << num;
```



```c++
#include <iostream>
using namespace std;

int main()
{
    cout << timeSpent("C++", 7);	//在main函数之外被定义
}

string timeSpent(string language, int hours)
{
    string log = "I've spent" + to_string(hours) + " hours with " + language;
    return log;
}
```



# 类

所有一切的事物都是object

- property
- behavior

```c++
class Dog
{
    public:
        string name;
        int age;
        string breed;
        string favoriteToy;
    	//constructor-实例init(不一定会有)：所以不需要返回类型(并非函数method)
    	//constructor参数的顺序最好与property的顺序相同
    	Dog(string givenName, int givenAge)	
        {
            name = givenName;
            age = givenAge;
        }
};

//实例化
Dog Fido("Fido", 3);
```



```c++
class Dog
{
    public:
    string name;
    
    void setAge(int num)	
    {
        age = num;
    }
    int getAge(void)
    {
        return age;
    }
    
    private:
    	int age;
}

Dog newDog;
newDog.setAge(5);
cout << newDog.getAge();
```



# IO

input stream：

- bytes序列(通常来自键盘)
- 输入到程序的内存

output stream：

- bytes序列
- 从程序到输出设备，比如屏幕或者打印机

```c++
#include <iostream>
using namespace std;

int main()
{
    string city;
    cout << "What city are you from?";
    cin >> city;
    cout << city << " is a great place!";
    clog << ... ;
    cerr << ... ;
}
```





# 模块

## header头文件

- 不必在程序开头就定义函数

- 包含类的声明(类中包含常用的函数)

- 建立到C++库的链接