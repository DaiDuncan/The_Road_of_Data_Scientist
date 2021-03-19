# iostream

提供了 cin 和 cout 方法分别用于从标准输入读取流和向标准输出写入流

```c++
//输出一个4*4的数组元素
cout<<std::setw(5);
cout<<"\n";
for(int i=0;i<4; i++)
{
    for(int j=0;j<4; j++)
    {
        cout<<Array[i][j];
    }
    cout<<"\n";
}
```



# fstream

从文件读取流和向文件写入流



==以内存的视角==来看output和input：

- output输出到文件：也就是向文件写入信息

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| ofstream | 该数据类型表示输出文件流，用于创建文件并向文件写入信息。     |
| ifstream | 该数据类型表示输入文件流，用于从文件读取信息。               |
| fstream  | 该数据类型通常表示文件流，且同时具有 ofstream 和 ifstream 两种功能，这意味着它可以创建文件，向文件写入信息，从文件读取信息。 |

步骤：

 - Include the <fstream> library 
 - Create a stream (input, output, both)
   - ofstream myfile; (for writing to a file)
   - ifstream myfile; (for reading a file)
   - fstream myfile; (for reading and writing a file)
 - Open the file  myfile.open(“filename”);
 - Write or read the file
 - Close the file myfile.close();



## 打开文件

**ofstream** 和 **fstream** 对象都可以用来打开文件进行**写操作**

如果只需要打开文件**进行读操作**，则使用 **ifstream** 对象



open() 函数是 fstream、ifstream 和 ofstream 对象的一个成员

```c++
void open(const char *filename, ios::openmode mode);
```

openmode：

- 可以把两种或两种以上的模式结合使用

| 模式标志   | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| ios::app   | 追加模式。所有写入都追加到文件末尾。                         |
| ios::ate   | 文件打开后定位到文件末尾。                                   |
| ios::in    | 打开文件用于读取。                                           |
| ios::out   | 打开文件用于写入。                                           |
| ios::trunc | 如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为 0 |

```c++
// 以写入模式打开文件，并希望截断文件，以防文件已存在
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc );

// 想要打开一个文件用于读写
ifstream  afile;
afile.open("file.dat", ios::out | ios::in );
```





## 操作|写入 outfile <<

C++ 编程中，我们使用流插入运算符（ << ）向文件写入信息，就像使用该运算符输出信息到屏幕上一样

唯一不同的是，在这里您==使用的是 **ofstream** 或 **fstream** 对象==，而不是 **cout** 对象



## 操作|读取 infile  >>

在 C++ 编程中，我们使用流提取运算符（ >> ）从文件读取信息，就像使用该运算符从键盘输入信息一样。唯一不同的是，在这里您使用的是 **ifstream** 或 **fstream** 对象，而不是 **cin** 对象





## 关闭文件

当 C++ 程序终止时，它会自动关闭刷新所有流，释放所有分配的内存，并关闭所有打开的文件。

但程序员应该养成一个好习惯，在程序终止前关闭所有打开的文件。



close() 函数是 fstream、ifstream 和 ofstream 对象的一个成员

```c++
void close();
```







## 文件位置指针

**istream** 和 **ostream** 都提供了用于重新定位文件位置指针的成员函数。

这些成员函数包括关于 istream 的 **seekg**（"seek get"）和关于 ostream 的 **seekp**（"seek put"）



seekg 和 seekp 的参数通常是一个长整型。

第二个参数可以用于指定查找方向。

查找方向可以是 

- **ios::beg**（默认的，从流的开头开始定位）
- 也可以是 **ios::cur**（从流的当前位置开始定位）
- 也可以是 **ios::end**（从流的末尾开始定位）。



文件位置指针是一个整数值，指定了从文件的起始位置到指针所在位置的字节数。

下面是关于定位 "get" 文件位置指针的实例：

```c++
// 定位到 fileObject 的第 n 个字节（假设是 ios::beg）
fileObject.seekg( n );
 
// 把文件的读指针从 fileObject 当前位置向后移 n 个字节
fileObject.seekg( n, ios::cur );
 
// 把文件的读指针从 fileObject 末尾往回移 n 个字节
fileObject.seekg( n, ios::end );
 
// 定位到 fileObject 的末尾
fileObject.seekg( 0, ios::end );
```





# 实例

## 1 先写后读取

- 注意需要声明ofstream和ifstream对象

```c++
#include <iostream>	//包含了ofstream和ifstream
#include <fstream>
#include <string>
using namespace std;

int main () {
    string line;
    
    // 写入文件内容
    //create an output stream to write to the file
    //append the new lines to the end of the file
    ofstream myfileI ("input.txt", ios::app);
    if (myfileI.is_open())
    {
        myfileI << "\nI am adding a line.\n";
        myfileI << "I am adding another line.\n";
        myfileI.close();
    }
    else cout << "Unable to open file for writing";

    
    // 读取文件内容
    //create an input stream to read the file
    ifstream myfileO ("input.txt");
    //During the creation of ifstream, the file is opened. 
    //So we do not have explicitly open the file. 
    if (myfileO.is_open())
    {
        while ( getline (myfileO,line) )
        {
            cout << line << '\n';
        }
        myfileO.close();
    }

    else cout << "Unable to open file for reading";

    return 0;
}
```



```c++
/* 修改为fstream */
...
fstream  myfileI ("input.txt", ios::app | ios::out);
...
fstream  myfileO ("input.txt");
```





## 2 先写后读取；复制

```c++
#include "iostream"
#include "fstream"
using namespace std;

//向文件内部写入数据，并将数据读出
void file_wr(void)
{
    char data[100];
    //向文件写入数据
    ofstream outfile;
    outfile.open("test.txt");
    cout << "Write to the file" << endl;
    cout << "Enter your name:" << endl;
    cin.getline(data, 100);
    outfile << data << endl;
    cout << "Enter your age:" << endl;
    cin >> data;
    cin.ignore();
    outfile << data << endl;
    outfile.close();
    //从文件读取数据
    ifstream infile;
    infile.open("test.txt");
    cout << "Read from the file" << endl;
    infile >> data;
    cout << data << endl;
    infile >> data;
    cout << data << endl;
    infile.close();
}


//将数据从一文件复制到另一文件中
void file_copy(void)
{
    char data[100];
    ifstream infile;
    ofstream outfile;
    infile.open("test.txt");
    outfile.open("test_1.txt");
    cout << "copy from test.txt to test_1.txt" << endl;
    while (!infile.eof())
    {
        infile >> data;
        cout << data << endl;
        outfile << data << endl;
    }
    infile.close();
    outfile.close();
}

//测试上述读写文件，与文件数据复制
int _tmain(int argc, _TCHAR* argv[])
{
    file_wr();
    file_copy();
    return 0;
}


/*
$./a.out
Writing to the file
Enter your name: 
John
Enter your age:
20
Reading from the file
John
20
copy from test.txt to test_1.txt
John
20
*/
```





## 3读写一个文件

以读写模式打开一个文件。

在向文件 afile.dat 写入用户输入的信息之后，程序从文件读取信息，并将其输出到屏幕上

- 实例中使用了 cin 对象的附加函数
  - 比如 getline()函数==从外部读取一行==
  - ignore() 函数会忽略掉==之前读语句留下的多余字符==。

```c++
#include <fstream>
#include <iostream>
using namespace std;
 
int main ()
{
    
   char data[100];
 
   // 以写模式打开文件
   ofstream outfile;
   outfile.open("afile.dat");
 
   cout << "Writing to the file" << endl;
   cout << "Enter your name: "; 
   cin.getline(data, 100);
 
   // 向文件写入用户输入的数据
   outfile << data << endl;
 
   cout << "Enter your age: "; 
   cin >> data;
   cin.ignore();
   
   // 再次向文件写入用户输入的数据
   outfile << data << endl;
 
   // 关闭打开的文件
   outfile.close();
 
   // 以读模式打开文件
   ifstream infile; 
   infile.open("afile.dat"); 
 
   cout << "Reading from the file" << endl; 
   infile >> data; 
 
   // 在屏幕上写入数据
   cout << data << endl;
   
   // 再次从文件读取数据，并显示它
   infile >> data; 
   cout << data << endl; 
 
   // 关闭打开的文件
   infile.close();
 
   return 0;
}

/* 输出结果
$./a.out
Writing to the file
Enter your name: Zara
Enter your age: 9
Reading from the file
Zara
9
*/
```



## 4 最简单的读取文件+ cin + cout

```c++
#include <fstream>
#include <iostream>

using namespace std;

int main(int argc, _TCHAR* argv[])
{
  char data[50];
  fstream file;
    
  file.open("file.txt",ios::ate|ios::app);//打开文件进行写入操作  定位到文件末尾
  cout<<"请输入你的姓名："<<endl;
  cin.getline(data,50);	//用户输入
  file<<data<<endl;//往file.txt里写入
    
  cout<<"请输入你的性别："<<endl;
  cin>>data;//重新定义数据
  cin.ignore();
  file<<data<<endl;
    
  file.close();//关闭文件 释放资源
    
    
  file.open("file.txt",ios::in);//打开文件进行读取操作
  file>>data;//从开始处读取到空格(多个空格两个以上)或换行结束
  cout<<data<<endl;
  file.close();
  system("pause");
}
```



# 补充

## cin.ignore()

完整版本是 cin.ignore(int n, char a), 从输入流 (cin) 中提取字符，提取的字符被忽略 (ignore)，不被使用。

每抛弃一个字符，它都要计数和比较字符：如果计数值达到 n 或者被抛弃的字符是 a，则 cin.ignore()函数执行终止；否则，它继续等待。

它的一个常用功能就是用来清除==以回车结束的输入缓冲区的内容==，消除上一次输入对下一次输入的影响。



比如可以这么用：cin.ignore(1024,'\n')，通常把第一个参数设置得足够大，这样实际上总是只有第二个参数 \n 起作用，所以这一句就是把回车(包括回车)之前的所有字符==从输入缓冲(流)中清除出去==。

```c++
#include <iostream>
using namespace std;
void main()
{
    int a,b,c;
    cout<<"input a:";
    cin>>a;
    cin.ignore(1024, '\n');
    cout<<"input b:";
    cin>>b;
    cin.ignore(1024, '\n');
    cout<< "input c:";
    cin>> c;
    cout<< a << "\t" << b << "\t" << c << endl;
}
```

如果cin.ignore()不给参数，则默认参数为cin.ignore(1,EOF) 其中 EOF是end of file的缩写，表示"文字流"(stream)的结尾。

- 只清除掉文字流的结尾标志

```c++
#include<iostream>  
using   namespace   std;  
int main()  
{  
    char   str1[30],str2[30],str3[30];  
    cout   <<   "请输入你的姓名：";  
    cin>>str1;  
    
    cout<<"请输入你的住址：";  
    cin.ignore();  
    cin.getline(str2,30,'a');  
    
    cout   <<   "请输入你的籍贯：";  
    cin.ignore();  
    cin.getline(str3,30);  
    cout<<str3;  
}
```

如果在地址那里输入 bcdabcd 那么此时流里面剩的是 bcd\n

- cin.getline(str2,30,'a')从'a'以后开始读取

此时 cin.ignore()，吃掉的就是b了，这是流里还剩下 cd\n 直接交给 cin.getline(str3,30); 

因为有个 \n 所以这里 getline 就直接返回。





补充：由于 **eof** 指示是在读取文件到结尾的时候，才会改变有效的状态。

但是，在下一次没有读到数据的时候，**eof** 才会改变



但是如果此时还是用 **eof** 标志位来判断文件是否读到了 **end**（下图while循环），就会导致此时的 **infile>>data** 语句还会流入数据（文件的最后一行数据），导致复制的文件中，会多出一行。

```c++
while (!infile.eof())
{
    infile >> data;
    cout << data << endl;
    outfile << data << endl;
}

/*按照 笔记1 中的输入
copy from test.txt to test_1.txt
John
20
20
*/
```

为消除多与的读取、复制，我们只需要在==还能读取文件数据的时候==(while条件)，才将数据复制到新文件中即可，即代码可以改成：

```c++
while (infile >> data)
{
    cout << data << endl;
    outfile << data << endl;
}
```





## **getline(cin，str)**

对于 **cin** 的操作 使用 **getline(cin，str)**往往可以实现更加简单以及安全的字符串操作,不同于 cin.getline(char*, int a)，前者可以直接对字符串进行操作

```c++
#include<iostream>
#include<string>
#include<fstream>
using namespace std;
int main()
{
    //可以读也可以写
    fstream iofile;
    iofile.open("D:\\t.txt", ios::out | ios::in | ios::trunc);
    
    string bookname;
    string bookwriter;
    
    cout << "input the bookname:" << endl;
    getline(cin, bookname);
    
    //读取文件内容到内存
    iofile << bookname << endl;
    
    cout << "input the bookwriter:" << endl;
    getline(cin, bookwriter);
    
    iofile << bookwriter << endl;
    iofile.close();
    
    
    cout << "read the input file:" << endl;
    iofile.open("D:\\t.txt");
    while (getline(iofile, bookname))
    {
        cout << bookname << endl;
    }
 
    system("pause");
    return 0;
}

/*
input the bookname:
we are the world	//用户输入
input the bookwriter:
Tanks				//用户输入
read the input file:
we are the world
Tanks
请按任意键继续. . .
*/
```







# 实用|Udacity⭐

1 特殊|在Udacity的环境中，读取txt文件的数据

```

```



2 C++如何从txt的某一行开始读取数据？

```c++
//定位到txt文件的某一行开始读取
ifstream & seek_to_line(ifstream & in, int line)
    
//将打开的文件in，定位到line行。
{
	int i;
	char buf[1024];
	in.seekg(0, ios::beg);  //定位到文件开始。
	for (i = 0; i < line; i++)
	{
		in.getline(buf, sizeof(buf));//读取行。
	}
	return in;
}
```







