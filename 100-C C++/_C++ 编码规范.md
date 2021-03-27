原文参见[Google 开源项目风格指南 (中文版)](https://link.zhihu.com/?target=http%3A//zh-google-styleguide.readthedocs.io/en/latest/)

> 目前编写50w行代码，维护314w行代码，整个项目1017万行代码。
>
> 血的教训告诉你，编码规范不应该是建议执行，而是需要严格执行，并且纳入员工考核的重要KPI指标，否则代码量会急剧膨胀到不可维护的阶段……然后整个公司就歇菜了。



## 6.3 类型命名

类型名称的每个单词首字母均大写, 不包含下划线: MyExcitingClass, MyExcitingEnum.

所有类型命名 —— 类, 结构体, 类型定义 (typedef), 枚举 —— 均使用相同约定. 例如:

```c++
// classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// enums
enum UrlTableErrors { ...
```







## 6.5 常量命名

在全局或类里的常量名称前加 k: kDaysInAWeek. 且除去开头的 k 之外每个单词开头字母均大写。

所有编译时常量, 无论是局部的, 全局的还是类中的, 和其他变量稍微区别一下. k 后接大写字母开头的单词:

```c++
const int kDaysInAWeek = 7;
```





## 6.6 函数命名⭐

常规函数使用大小写混合, 取值和设值函数则要求与变量名匹配: MyExcitingFunction(), MyExcitingMethod(), my_exciting_member_variable(), set_my_exciting_member_variable().



### 常规函数

函数名的每个单词首字母大写, 没有下划线。

如果您的某函数出错时就要直接 crash, 那么就在函数名加上 OrDie. 但这函数本身必须集成在产品代码里，且平时也可能会出错。

```c++
AddTableEntry()
DeleteUrl()
OpenFileOrDie()
```



### 取值和设值函数

取值（Accessors）和设值（Mutators）函数要与存取的变量名匹配. 这儿摘录一个类, num_entries_ 是该类的实例变量:

```c++
class MyClass {
    public:
        ...
        int num_entries() const { return num_entries_; }
        void set_num_entries(int num_entries) { num_entries_ = num_entries; }

    private:
        int num_entries_;
};
```

其它非常短小的内联函数名也可以用小写字母, 例如. 如果你在循环中调用这样的函数甚至都不用缓存其返回值, 小写命名就可以接受.





