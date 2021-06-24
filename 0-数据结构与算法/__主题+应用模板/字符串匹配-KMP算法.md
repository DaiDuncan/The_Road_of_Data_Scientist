## 背景

leetcode `28 strStr()`

Accepted

- 79/79 cases passed (24 ms)
- Your runtime beats 23.01 % of cpp submissions
- Your memory usage beats 22.57 % of cpp submissions (7 MB)

```c++
#include<iostream>
#include<string>

class Solution {
public:
    int strStr(string haystack, string needle) {
        if (needle.length()==0){
            return 0;
        }


        int size = needle.length()+1;
        int next[size]; //动态数组的生成：大小改变后，但不能直接初始化
        next[0]=0;
        int j = 0;

        /* step2 遍历匹配 */
        for (int i = 1; i < needle.length(); i++){        //隐含条件：j==0的时候，不满足下列素有判断条件，仍要i++
            /* step2.1 不匹配 */
            while(j > 0 && needle[i] != needle[j]){	//这个while很有道理：一直回退到0位置，
                j = next[j-1];
            }
            if (needle[i] == needle[j]){
                ++j;
            }
            next[i] = j;	//包含i=0的情况
        }

        for(int num=0; num < needle.length(); num++) {
            std::cout<<next[num]<<" ";
        }

        /* 2)next数组移位 */
        for(int i=needle.length()-1; i>0; i--) {    //注意判断条件
            next[i] = next[i-1];
        }
        next[0] = -1;
        
        int i=0;
        j=0;

        while (i < haystack.length() && j < needle.length()) {        
        /* step2.1 字符不匹配：移位 */
            if (haystack[i] != needle[j]) {
                if (j==0){
                    ++i;
                }else{
                    //如果匹配函数没有向后移位，那就是j=next[j-1]
                    j = next[j];	//i不变，j后退
                }   
            }
            /* step2.2 字符匹配 */
            else{
                ++i;
                ++j;
            }   
        }
        /* step3 结束判断 */

        if (j == needle.length()) {
            return i - j;
        }
        return -1;
    }
};
```



## 理解后优化⭐

Accepted

- 79/79 cases passed (4 ms)
- Your runtime beats 77.04 % of cpp submissions => ==KMP算法还是有用的==
- Your memory usage beats 32.38 % of cpp submissions (6.8 MB)

```c++
#include<iostream>
#include<string>

class Solution {
public:
    int strStr(string haystack, string needle) {
        int haystackLen = haystack.length();
        int needleLen = needle.length();

        if (needleLen == 0){
            return 0;
        }

        
        
        /* 获取next数组 */
        int next[needleLen]; //动态数组的生成：大小改变后，但不能直接初始化
        next[0]=-1;
        int k = -1;  //前缀
        int j = 0;  //后缀
        
        while (j < needleLen - 1){
			//[k]表示前缀，[j]表示后缀
            if (k == -1 || needle[j] == needle[k]) 
            {
                ++k;
                ++j;
                next[j] = k;
            }
            else 
            {
                k = next[k];
            }
	    }
        


        /* KMP算法 */
        int i=0;
        j=0;

        while (i < haystackLen && j < needleLen) {  
            //①如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++    
            if (j == -1 || haystack[i] == needle[j]){
                i++;
                j++;
            }
            else{
            //②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]    
            //next[j]即为j所对应的next值：相当于模式串向右移动的位数为j - next[j]    
                j = next[j];
            }
        }


        if (j == needleLen) {
            return i - j;
        }
        return -1;
    }
};
```



## 【问题】

### 1 数组的初始化⭐👌

leetcode反馈：原先长度255，但是输入的字符串长度超过255

怎么办？

- 如果直接用 `int next[size]={0}; `会报错

```c++
int size = needle.length()+1;
int next[size]; //动态数组的生成：大小改变后，但不能直接初始化
next[0]=0;
```

[link: stackoverflow](https://stackoverflow.com/questions/3082914/c-compile-error-variable-sized-object-may-not-be-initialized)

数组的初始化：

> 3 The type of the entity to be initialized shall be an array of unknown size or an object type that is not a variable length array type.

```c++
//error
int len;
scanf("%d",&len);
char str[len]="";

//error
int len=5;
char str[len]="";


//correct
int len=5;
char str[len]; //so the problem lies with assignment not declaration
str[0]='a';
str[1]='b'; //like that; and not like str="ab";
```



```c++
int boardAux[length][length];

int i, j;
for (i = 0; i<length; i++)
{
    for (j = 0; j<length; j++)
        boardAux[i][j] = 0;
}
```



需要预编译：==数组的大小应该在编译期间确定，而不是运行期间==

```c++
#define length 10

int main()
{
    int boardAux[length][length] = {{0}};
}
```



### 2 如何获取数组的长度？👌

```c++
sizeof(array) / sizeof(array[0])
```



### 3 将独立函数转化为类

1 在类中调用类的方法？



2 变量的重复定义

- for循环变量在是局部变量，for循环结束后销毁



### 4 数组传递到函数中⭐

- 引用数组名

- 以指针的形式



## 【技巧】

- `int main(){}`
- `j==-1`的巧妙处理
  - 判断：或的条件
  - 同时移位 => 模式串的字符



# me

## getNext数组

[link: B站|实现KMP算法中的next数组](https://www.bilibili.com/s/video/BV1M5411j7Xx)

- 注意：next数组有不同的表示方式
  - 原始：只要在KMP主算法中改为j=next[j]即可 => 用起来更方便 ⭐
  - 向右移位 + `next[0] = -1` 👌(实现移位过程)



初始思路(很粗糙)

```c++
void GetNext(string child, int *next) {
    /* 1）部分匹配：建议用模式串 aafaaff 来分析 */
	/* step1 初始化 */
    //i在for循环中初始化
    int j = 0;
    next[0] = 0;   //最初的部分匹配表
    
    /* step2 遍历匹配 */
    for (int i = 1; i < child.length(); i++){        //隐含条件：j==0的时候，不满足下列素有判断条件，仍要i++
        /* step2.1 不匹配 */
        //这个while很有道理：一直回退到0位置
        while(j > 0 && child[i] != child[j]){	
            j = next[j-1];
        }
        /* step2.2 匹配 */
        if (child[i] == child[j]){
            ++j;
        }
        /* step3 更新next数组 */
        next[i] = j;	//包含i=0的情况
    }

    /* 2)next数组移位 */
    for(int i = child.length()-1; i > 0; i--) {	//注意数组移位的方向，更改判断条件
        next[i] = next[i-1];
    }
    next[0] = -1;
	
    //\\ 测试：打印输出
    for(int num=0; num < child.length(); num++) {
    std::cout<<next[num]<<" ";
    }
}
```



⭐博客优化: 

- 不用优化
- 思路有更多技巧：需要结合过程进行揣摩(上述思路简单、粗暴)

```c++
void GetNext(char* p,int next[])
{
	int pLen = strlen(p);
	next[0] = -1;
	int k = -1;
	int j = 0;
	while (j < pLen - 1)
	{
		//p[k]表示前缀，p[j]表示后缀
		if (k == -1 || p[j] == p[k]) 
		{
			++k;
			++j;
			next[j] = k;
		}
		else 
		{
			k = next[k];
		}
	}
}
```



## KMP匹配

初始思路(很粗糙)

```c++
/*KMP匹配算法*/
int KMPCompare(string parent, string child, int pos) {	//int pos=1表示从文本首字符开始匹配
    if (needle.length()==0){
            return 0;
    }
    
    /* step1 初始化 */
    int next[255]={0};	//数组的初始化
    GetNext(child, next);	//next是数组传递

    int i = pos - 1;	//输入i=1
    int j = 0;     

    while (i < parent.length() && j < child.length()) {        
        /* step2.1 字符不匹配：移位 */
        if (parent[i] != child[j]) {
            if (j==0){
                ++i;
            }else{
                //如果匹配函数没有向后移位，那就是j=next[j-1]
                j = next[j];	//i不变，j后退
            }   
        }
        /* step2.2 字符匹配 */
        else{
            ++i;
            ++j;
        }
    }
    /* step3 结束判断 */
    if (j == child.length()) {
        return i - j;
    }
    return -1;
}
```



⭐博客原版：

- 因为`next[j]=0`，说明

```c++
/*KMP匹配算法*/⭐
int KmpSearch(char* s, char* p)
{
	int i = 0;
	int j = 0;
	int sLen = strlen(s);
	int pLen = strlen(p);
	while (i < sLen && j < pLen)
	{
		//①如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++    
		if (j == -1 || s[i] == p[j])
		{
			i++;
			j++;
		}
		else
		{
			//②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]    
			//next[j]即为j所对应的next值：相当于模式串向右移动的位数为j - next[j]    
			j = next[j];
		}
	}
	if (j == pLen)
		return i - j;
	else
		return -1;
}


//============================
//============================
//更改部分
if (parent[i] != child[j]) {
    if (j==0){
        ++i;
    }else{
        //如果匹配函数没有向后移位，那就是j=next[j-1]
        j = next[j];	//i不变，j后退
    }   
}
/* step2.2 字符匹配 */
else{
    ++i;
    ++j;
}

//博客原版
if (j == -1 || s[i] == p[j])	//j==-1，只有在j等于0且未匹配的情况下成立 => next[0]=-1
{
    i++;
    j++;
}
else
{
    //②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]    
    //next[j]即为j所对应的next值：相当于模式串向右移动的位数为j - next[j]    
    j = next[j];
}


```



~~公众号|错误示例~~

```c++
/*KMP匹配算法*/
int KMPCompare(HString parent, HString child, intpos) {
    int next[255];
    Get_Next(child, next);
    int i = pos - 1;
    int j = 0;     //j用于子串child中的起始位置
    while (i < parent.length && j < child.length) {
        if (j == 0 || parent.ch[i] == child.ch[j]) {	//错误：不应该j==0 对比上述优化代码
            ++i;
            ++j;
        }
        else {
            j = next[j];    //i不变，j后退
        }
    }
    if (j == child.length) {
        return (i + 1) - j;
    }
    return 0;
}
```





## main()


```c++
#include<iostream>
#include<string>

using namespace std;

void GetNext(string child, int *next);
int KMPCompare(string parent, string child, int pos);	//int pos=1表示从文本首字符开始匹配


int main() {
    /*使用KMP算法匹配串*/
    string parent, child;
    parent = "hello";    //"BBC ABCDAB ABCDABCDABDE"
    child = "ll";     //"ABCDABD"
    //parent = "BBC ABCDAB ABCDABCDABDE";    //结果是15
    //child = "ABCDABD";     //

    std::cout << "Index = %d\n" << KMPCompare(parent, child, 1) << std::endl;
    return -999;
}
```

