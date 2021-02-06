python这门语言中，True和1是相等的, False和0是相等的

```python
print(True==1)
print(False==0)
```

究其根本，bool类型是int类型的子类

```python
print(issubclass(bool, int))	#True
```



# 注意1：Ture 和 1不能同时作为字典的key

字典不允许key重复，True: 'True'这个key-value对覆盖了前面的，不只是不能同时出现在字典里做key，他们也不能同时出现在集合中，道理是一样的



bool类型的数据，虽然可以做字典的key，但bool类型本身也只有两个常量数值，因此，很不建议你用bool类型数据做key，即便没有1做key







# 注意2：Ture 和 1相等，导致统计不准确

```python
lst = [1, True]

for item in lst:
    if item == 1:
        print(item)	#结果为2
        
### 等价于
print(lst.count(1))	#结果为2
```



```python
### 先判断所遍历数据不是bool类型
lst = [1, True]

for item in lst:
    if not isinstance(item, bool) and item == 1:
        print(item)
```