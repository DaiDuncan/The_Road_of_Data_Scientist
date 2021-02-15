```c++
namespace std {
template <class Type, class Allocator>
class vector;
template <class Allocator>
class vector<bool>;

template <class Allocator>
struct hash<vector<bool, Allocator>>;

// TEMPLATE FUNCTIONS
template <class Type, class Allocator>
bool operator== (
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
bool operator!= (
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
bool operator<(
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
bool operator> (
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
bool operator<= (
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
bool operator>= (
    const vector<Type, Allocator>& left,
    const vector<Type, Allocator>& right);

template <class Type, class Allocator>
void swap (
    vector<Type, Allocator>& left,
    vector<Type, Allocator>& right);

}  // namespace std
```

将给定类型的元素组织到线性序列中的容器。 它使用户可以快速随机访问任何元素，并动态添加到序列和动态从序列中删除。 `vector` 是随机访问性能超出限制时的首选序列容器。



## 参数Parameters

| 参数      |                          |
| --------- | ------------------------ |
| Type      | 数据类型                 |
| Allocator | 内存对象：内存分配与回收 |
| left      |                          |
| right     |                          |



## 成员Members

### 运算符Operators

| 名称                                                         | 描述                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------- |
| [操作员!=](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_neq) | 测试运算符左侧的向量对象是否等于右侧的向量对象。       |
| [运算符<](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_lt) | 测试运算符左侧的向量对象是否小于右侧的向量对象。       |
| [操作员<=](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_gt_eq) | 测试运算符左侧的向量对象是否小于或等于右侧的向量对象。 |
| [operator = =](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_eq_eq) | 测试运算符左侧的向量对象是否等于右侧的向量对象。       |
| [运算符>](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_gt) | 测试运算符左侧的向量对象是否大于右侧的向量对象。       |
| [运算符>=](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-operators?view=msvc-160#op_gt_eq) | 测试运算符左侧的向量对象是否大于或等于右侧的向量对象。 |







### 类Classes

| “属性”                                                       | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [vector 类](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-class?view=msvc-160) | 序列容器的一个类模板，它将给定类型的元素以线性排列方式进行排列，并且允许快速随机访问任何元素。 |



## 专用

| 名称                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| hash                                                         | 返回向量的哈希值。                                           |
| [vector  类](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector-bool-class?view=msvc-160) | 针对类型的元素的类模板向量的完全专用化，它 **`bool`** 具有专用化所使用的基础类型的分配器。 |







# 要求

- **标头：**<vector>
- **命名空间:** std





# #参考文献

[Link: 微软|<vector>](https://docs.microsoft.com/zh-cn/cpp/standard-library/vector?view=msvc-160)

