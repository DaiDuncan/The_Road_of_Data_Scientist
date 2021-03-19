link: CSDN|vector用法总结](https://blog.csdn.net/sevenjoin/article/details/81901259)

可以容纳许多类型的数据，如若干个整数，所以称其为容器

相当于一个动态的数组，当程序员无法知道自己需要的数组的规模多大时，用其来解决问题可以达到最大节约空间的目的。

vector 是C++ STL的一个重要成员，使用它时需要包含头文件：

```c++
 #include<vector>
```

## 背景

Object oriented library：面向对象类的STL

- 包含大量容器：vector class => ==本质：是一种数据类型type==

```c++
// constructing vectors
#include <iostream>
#include <vector>  //Need to include the vector library!

int main ()
{
  //creating a vector of integers
  std::vector<int> vectorInts;  
  std::cout<<"vectorInts has "<<vectorInts.size()<<" elements\n";	//初始化的时候size是0
  
  //Changing the size of vectorInts to 6
  vectorInts.resize(6);
  std::cout<<"\n\nvectorInts now has "<<vectorInts.size()<<" elements\n";
 
  return 0;
}
```



## 特性|迭代器

vector比数组有更多有点，而且在运行期间可以调整容器大小

- 注意第一个元素不是0，而被称为beginning，最后一个元素是end



作用：用迭代器遍历vector

@迭代器-实例化

```c++
//creating an iterator for the vector
std::vector<int>::iterator it;
```



@迭代器-遍历

```c++
for (it = vectorInts.begin(); it != vectorInts.end(); ++it)	//迭代器自增
    std::cout<<*it<<" ";	//*是dereference，表示内存指向的值 value
```



### 示例：

```c++
#include <iostream>
#include <vector>


int main ()
{
  //creating a vector of integers
  std::vector<int> vectorInts;  
  //creating an iterator for the vector
  std::vector<int>::iterator it;
  
  std::cout<<"vectorInts has "<<vectorInts.size()<<" elements\n";
  
  std::cout<<"\n\nAdding four elements to the vector\n";
  //assigning the value 3 to 4 elements of the vector
  vectorInts.assign(4,3);
  std::cout<<"vectorInts has "<<vectorInts.size()<<" elements\n";
  
  //printing the contents of vectorInts
  std::cout<<"VectorInts has these elements:\n";
  for (it = vectorInts.begin(); it != vectorInts.end(); ++it)
    std::cout<<*it<<" ";

  return 0;
}
```



## 操作1|创建

```c++
vector();
vector(size_type num, const TYPE &val);
vector(const vector &from);
vector(input_iterator start, input_iterator end);
```

变量声明：

1 声明一个int向量以替代一维的数组 `vector <int> a;`(等于声明了一个int数组a[],大小没有指定,可以动态的向里面添加删除)

2 用vector代替二维数组.其实只要声明一个一维数组向量即可,而一个数组的名字其实代表的是它的首地址,所以只要声明一个地址的向量即可

- `vector <int *> a`
- `vector<vector<int>> vec`

用向量代替三维数组也是一样 `vector <int**>a;`，再往上面依此类推

```c++
vector<int> a;           //无参数 - 构造一个空的vector,
vector<int> a(10);       //定义了10个整型元素的向量（尖括号中为元素类型名，它可以是任何合法的数据类型），但没有给出初值，其值是不确定的。
vector<int> a(10,1);     //定义了10个整型元素的向量,且给出每个元素的初值为1
vector<int> a(b);        //用b向量来创建a向量，整体复制性赋值， 拷贝构造
vector<int> v3=a ;       //移动构造
vector<int> a(b.begin(),b.begin+3);   //定义了a值为b中第0个到第2个（共3个）元素
int b[7]={1,2,3,4,5,9,8};
```



### 复制

```c++
copy(a.begin(),a.end(),b.begin()+1); //把a中的从a.begin()（包括它）到a.end()（不包括它）的元素复制到b中，从b.begin()+1的位置（包括它）开        始复制，覆盖掉原有元素
```



## 操作2|查 选/索引(含切片) 改 增 删

### 查

```c++
vector<int> a(b,b+6);    //从数组中获得初值，b[0]~b[5]

a[i];                 //返回a的第i个元素，当且仅当a[i]存在2013-12-07

a.back();             //返回a的最后一个元素
a.front();            //返回a的第一个元素

find(a.begin(),a.end(),10); //在a中的从a.begin()（包括它）到a.end()（不包括它）的元素中查找10，若存在返回其在向量中的位置
```



### 改

```c++
a.resize(10);        //将a的现有元素个数调至10个，多则删，少则补，其值随机
a.resize(10, 2);      //将a的现有元素个数调至10个，多则删，少则补，其值为2
a.reserve(100);      //将a的容量（capacity）扩充至100，也就是说现在测试a.capacity();的时候返回值是100.这种操作只有在需要给a添加大量数据的时候才显得有意义，因为这将避免内存多次容量扩充操作（当a的容量不足时电脑会自动扩容，当然这必然降低性能） 
```



### 增

```c++
//初始化增加
a.assign(b.begin(), b.begin()+3); //b为向量，将b的0~2个元素构成的向量赋给a
a.assign(4,2);        //是a只含4个元素，且每个元素为2


a.push_back(5);      //在a的最后一个向量后插入一个元素，其值为5

//insert的第一个参数是iterator,iterator的位置在一次插入之后，变得无效
a.insert(a.begin()+1, 5);         //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
a.insert(a.begin()+1, 3,5);       //在a的第1个元素（从第0个算起）的位置插入3个数，其值都为5
a.insert(a.begin()+1,b+3, b+6);   //b为数组，在a的第1个元素（从第0个算起）的位置插入b的第3个元素到第5个元素（不包括b+6），如b为1,2,3,4,5,9,8，插入元素后为1,4,5,9,2,3,4,5,9,8


a.emplace(a.begin()+1, 5);         //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
```

[link: insert与emplace的区别](https://stackoverflow.com/questions/14788261/c-stdvector-emplace-vs-insert)

emplace是C++ 11的特性

- `insert` copies objects into the vector.
- `emplace` *construct* them inside of the vector.

```c++
struct Foo
{
  Foo(int n, double x);
};

std::vector<Foo> v;
v.emplace(someIterator, 42, 3.1416);	//emplace接收对应的参数构造一个对象
v.insert(someIterator, Foo(42, 3.1416));	//直接引用对象
```



### 删

```c++
a.clear();            //清空a中的元素
a.empty();            //判断a是否为空，空则返回ture,不空则返回false

a.pop_back();         //删除a向量的最后一个元素：没有返回值

a.erase(a.begin()+1, a.begin()+3);  //删除a中第1个（从第0个算起）到第2个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+3（不包括它）
```





## 操作3|遍历 排序 统计

```c++
a.size();            //返回a中元素的个数；
a.capacity();        //返回a在内存中总共可以容纳的元素个数

sort(a.begin(),a.end());     //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素进行从小到大排列

reverse(a.begin(),a.end()); //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素倒置，但不排列，如a中元素为1,3,2,4,倒置后为4,2,3,1



```



## 补充|重要操作

```c++
a.swap(b);           //b为向量，将a中的元素和b中的元素进行整体性交换
a.begin();           // 返回指向容器第一个元素的迭代器
a.end();             // 返回指向容器最后一个元素的迭代器
a==b;                //b为向量，向量的比较操作还有!=,>=,<=,>,<
```







# C++11 vector的新增特性

c++11 对vector的扩展主要有:

```c++
a.cbegin();    // 返回指向容器中第一个元素的const_iterator
a.cend();      // 返回指向容器中最后一个元素的const_iterator
a.crbegin();   // 反转迭代器, 返回指向容器中最后一个元素的const_iterator
a.crend();     // 反转迭代器, 返回指向容器中第一个元素的const_iterator
a.emplace();   // 相对于insert功能，但比它更有效率
a.emplace_back(); //相对于push_back, 但比它更有效率
```

emplace_back能通过参数构造对象，不需要拷贝或者移动内存，相比push_back能更好地避免内存的拷贝与移动，使容器插入元素的性能得到进一步提升。 
　　

由此，在大多数情况下应该优先使用emplace_back来代替push_back







# #参考文献

[link: 官网|C++ vector](http://www.cplusplus.com/reference/vector/vector/vector/)