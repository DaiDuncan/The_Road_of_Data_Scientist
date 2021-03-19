隐式数据类型：

- 该数据类型可以是string, int, float等：视输入而定



## 背景

overloading可以利用同一个函数(功能)，但是需要根据每一次参数的不同情况，多次编写函数

---

## generic programming 通用编程

- 类型通用 => Templates

vector of T：(T是参数)

- int
- double
- string
- string-vector



# Templates

```c++
//示例：比较大小
template <typename T>
T findSmaller(T  input1,T  input2)
{
    if(input1 < input2)
        return input1;
    else
        return input2;
}
```



声明：

```c++
template <typename T>  //tell the compiler we are using a template

//T represents the variable type. Since we want it to be for any type, we 
//use T
T  functionName (T parameter1,T parameter2, ...); 
```

定义：

```c++
template <typename T>
T functionName (T  parameter1,T  parameter2,...)
{
    function statements;
}
```



## 多个不同类型的template

```c++
template <typename T, typename U, typename V>
T functionName (U  parameter1, V  parameter2,...)
{
    function statements;
}
```



实例|意义：输入两个不同类型的数据，哪一个变量出现在列表前面，就输出该变量

```c++
#include<iostream>
using namespace std;

template <typename T, typename U>
T getBigger(T input1, U input2);


int main()
{
    int a = 5;
    float b = 6.334;
    int bigger;
    cout<<"Between "<<a<<" and "<<b<<" "<<getBigger(a,b)<<" is bigger.\n";

    cout<<"Between "<<a<<" and "<<b<<" "<<getBigger(b,a)<<" is bigger.\n";    
    return 0;
}

template <typename T, typename U>
T getBigger(T input1, U input2)
{
    if(input1 > input2)
        return input1;
    return input2;
}
```



# generic classes 通用类

注意如果要接收string类型，需要添加`using namespace std;`

```c++
//main.hpp

//header file for main.cpp
#include<iostream>

//The class accepts strings, so we need to use namespace
using namespace std;


//tell compiler this class uses a generic value
template <class T>
class StudentRecord
{
    private:
        const int size = 5;
        T grade;
        int studentId;
    public:
       //note: I used a constructor that accepts the grade input
        StudentRecord(T input);
        void setId(int idIn);
        void printGrades();
};


//member functions: 都要添加template格式 StudentRecord<T> => 告诉编译器，我这个类用了template
template<class T>
StudentRecord<T>::StudentRecord(T input)
{
    grade=input;
}

//Notice I still add the template<class T here, even though this is not a generic //function. 
//关键点：It is in a generic class. 
template<class T>
void StudentRecord<T>::setId(int idIn)
{
    studentId = idIn;
}

template<class T>
void StudentRecord<T>::printGrades()
{
    cout<<"ID# "<<studentId<<": ";
    cout<<grade<<"\n ";
    cout<<"\n";
}
```





示例|main.cpp

```c++
#include "main.hpp"

int main()
{
    //StudentRecord is the generic class
    //The constructor sets the grade value
    StudentRecord<int> srInt(3);
    srInt.setId(111111);
    srInt.printGrades();
 
    StudentRecord<char> srChar('B');
    srChar.setId(222222);
    srChar.printGrades();

    StudentRecord<float> srFloat(3.333);
    srFloat.setId(333333);
    srFloat.printGrades();
    
    StudentRecord<string> srString("B-");
    srString.setId(4444);
    srString.printGrades();
    
    return 0;
}


//输出结果
ID# 111111: 3
 
ID# 222222: B
 
ID# 333333: 3.333
 
ID# 4444: B-
```



## 练习

```c++
//input.txt
-9
3
2.0001
56
```



```c++
//main.cpp

/*Goal: create a generic class.
**Create a class called Multiplier. 
**It multiplies two numbers - integers
**or floats. */

#include "main.hpp"

int main()
{
    Multiplier<int> multi1;
    Multiplier<float> multi3;
    
    int input1,input2;
    cin>>input1;
    cin>>input2;
    
    multi1.setM1(input1);
    multi1.setM2(input2);
    multi1.setProduct();
    multi1.printEquation();
    
    cout<<"\n";
    float input3, input4;
    cin>>input3;
    cin>>input4;    
    multi3.setM1(input3);
    multi3.setM2(input4);
    multi3.setProduct();
    multi3.printEquation();
    return 0;
}
```



```c++
#include<iostream>

using namespace std;

template <class T>
class Multiplier
{
    T m1, m2;
    T product;
public:
    void printEquation();
    void setM1(T mIn);
    void setM2(T mIn);
    void setProduct();
    T getProduct();
    T getM1();
    T getM2();
};

template <class T>
void Multiplier<T>::printEquation()
{
    std::cout<<m1<<" * "<<m2<<" = "<<product;
}

template <class T>
void Multiplier<T>::setM1(T mIn)
{
   m1 = mIn; 
}

template <class T>
void Multiplier<T>::setM2(T mIn)
{
   m2 = mIn; 
}

template <class T>
void Multiplier<T>::setProduct()
{
   product = m1 * m2; 
}

template <class T>
T Multiplier<T>::getM1()
{
   return m1; 
}

template <class T>
T Multiplier<T>::getM2()
{
   return m2; 
}

template <class T>
T Multiplier<T>::getProduct()
{
   return product; 
}
```





# Issue：与数组array

为了给数组分配内存，编译器需要知道：

- 数组的元素类型
- 数组的元素个数

## 情形1）区分类的实例化 VS 函数声明

```c++
//header file for main.cpp

#include<iostream>

using namespace std;
const int SIZE=5;

template <class T>
class StudentRecord
{
    private:
        const int size = SIZE;
        T grades[SIZE];
        int studentId;
    public:
        void setGrades(T* input);   //参数input[]是：分数数组
        void setId(int idIn);
        void printGrades();
};

template<class T>
void StudentRecord<T>::setGrades(T* input)
{
    for(int i=0; i<SIZE;++i)
    {
        grades[i] = input[i];
    }
}

template<class T>
void StudentRecord<T>::setId(int idIn)
{
    studentId = idIn;
}

template<class T>
void StudentRecord<T>::printGrades()
{
    std::cout<<"ID# "<<studentId<<": ";
    for(int i=0;i<SIZE;++i)
        std::cout<<grades[i]<<"\n ";
    std::cout<<"\n";
}
```



main.cpp

```c++
/*Goal: understand an
**issue with memory allocation
**in generic classes
*/
/***This code will not compile without errors*****/
#include "main.hpp"

int main()
{
    //StudentRecord is the generic class
    StudentRecord<int> srInt;	//错误版为StudentRecord<int> srInt()
    srInt.setId(111111);
    int arrayInt[SIZE]={4,3,2,1,4};
    srInt.setGrades(arrayInt);
    srInt.printGrades();
 
    StudentRecord<char> srChar;
    srChar.setId(222222);
    char arrayChar[SIZE]={'A','B','C','D','F'};
    srChar.setGrades(arrayChar);
    srChar.printGrades();
   
    StudentRecord<float> srFloat;
    srFloat.setId(333333);
    float arrayFloat[SIZE]={2.75,4.0,3.7,2.8,3.99};
    srFloat.setGrades(arrayFloat);
    srFloat.printGrades();
    
    StudentRecord<string> srString;
    srString.setId(4444);
    string arrayString[SIZE]={"B","B-","C+","B","A"};
    srString.setGrades(arrayString);
    srString.printGrades();
    
    return 0;
}
```

如果使用错误版 => 类中变量会报错

> **成员变量setId**的数据类型报错：
>
> error: request for member ‘setId’ in ‘srInt’, which is of **non-class type ‘StudentRecord<int>()’**     srInt.setId(111111);
>
> ​	which指的是'srInt'
>
> 
>
> 意思就是说：srInt只是某个函数，返回类型是StudentRecord<int>，而不是类的对象。
>
> 所以需要基于srInt这个函数，建立与setId的成员关系，才能使用srInt.setId



me|理解：==class本质上就是一个type==

- 所以`StudentRecord<int>`等价于type
- 而`StudentRecord<int> srInt();` 也被编译器理解为函数的声明





### 参考理解及解决办法

[link: 参考CSDN|错误问题的背景](https://blog.csdn.net/ninesnow_c/article/details/107190929)

- 在main函数中使用了`T_stack<int> s()`导致的
- 因为不带参数的构造函数，比如`T_stack<T>()`它可以是一个对象，==也可以是一个函数声明==。
  - 但是c++编译器总是优先认为是一个函数声明，然后是对象。

避免这种模糊性的方法：

1. 不使用()初始化对象，比如`T_stack<int> s`

2. 使用花括号{}初始化对象,比如`T_stack<int> s{};`
3. 使用赋值运算符和匿名默认构造的对象:`T_stack<int> s = T_stack<int> {}`。



[link: 参考CSDN|C++ 编译器把不带参数的构造函数优先认为是一个函数声明](https://blog.csdn.net/weixin_43742643/article/details/113850642?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1328641.24636.16156260298655765&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

解决办法：

1. 不使用 () 初始化对象 `ClassName obj;`
2. 使用 {} 初始化对象  `ClassName obj{};`





## 情形2）针对数组的内存分配

类的构造器：给data分配初始值 => 完成内存分配

```c++
//构造器
//header file for main.cpp

#include<iostream>

using namespace std;

const int SIZE = 5;
template <class T>
class StudentRecord
{
    private:
        const int size = SIZE;
        T grades[SIZE];
        int studentId;
    public:
        StudentRecord(T defaultInput);//A default constructor with a default value
        void setGrades(T* input);
        void setId(int idIn);
        void printGrades();
};

template<class T>
StudentRecord<T>::StudentRecord(T defaultInput)
{
    //we use the default value to allocate the size of the memory
    //the array will use
    for(int i=0; i<SIZE; ++i)
        grades[i] = defaultInput;

}
```





main.cpp：实例化

```c++
/*Goal: study generic classes
**Fix the program by completing
**the constructor. It should 
**assign a default value to each
**element in the array.*/

#include "main.hpp"

int main()
{
    //StudentRecord is the generic class
    //The constructor sets the grade value
    StudentRecord<int> srInt(-1);//add a default value using a constructor
    srInt.setId(123456);
    int arrayInt[SIZE]={0,0,0,0};
    srInt.setGrades(arrayInt);
    srInt.printGrades();
 
    StudentRecord<char> srChar('U');//add a default value using a constructor
    srChar.setId(234567);
    char arrayChar[SIZE]={'F','F','F','F','E'};
    srChar.setGrades(arrayChar);
    srChar.printGrades();
   
    StudentRecord<float> srFloat(-1.0);//add a default value using a constructor
    srFloat.setId(345678);
    float arrayFloat[SIZE]={2.75,4.0,3.7,2.8,3.99};
    srFloat.setGrades(arrayFloat);
    srFloat.printGrades();
    
    StudentRecord<string> srString("U");//add a default value using a constructor
    srString.setId(456789);
    string arrayString[SIZE]={"B","B-","C+","B","A"};
    srString.setGrades(arrayString);
    srString.printGrades();
    
    return 0;
}
```





# #参考文献

[link: 官网|templates](http://www.cplusplus.com/doc/oldtutorial/templates/)