[2020.01.17](https://mp.weixin.qq.com/s/Z077eD-GvmKilA3d1XOuVA)

北京时间2019年10月15日，Python 官方发布了 3.8 版本，新的 Python 3.8 版本有哪些必须知道的新特性？

- 2020年1月1日起，Python 2 将不再得到官方支持，这也基本宣告了它的死亡。



# 1 赋值表达式——可读性

新的运算符 := 被称为海象运算符，因为 := 很像小眼睛长牙齿的海象。

它能让你**把一行语句中的某一个表达式赋值给一个变量，**同时不影响该语句的原始逻辑。

```python
a = 6
# The following statement
# assigns the value a ** 2 to variable b, 
# and then check if b > 0 is true
if (b := a ** 2) > 0: 
    print(f'The square of {a} is {b}.') # The square of 6 is 36.
```

这样的赋值语句可以让你的代码更加紧凑，同时保持良好的可读性。但注意不要滥用它，否则在某些情况下可能会让你的代码变得更加难懂：

```python
# DON'T DO THIS!
a = 5
d = [b := a+1, a := b-1, a := a*2]
```







# 2 参数类型——可靠性

在 Python 中，一个函数可以接受两种不同方式指定的参数：

- **位置参数：**按其传入的顺序赋值给对应位置的参数；
- **关键字参数：**依据给定的关键字赋值给对应的参数。



新版本的 Python 3 提供了一个额外的语法糖，用来指明某些参数必须使用仅限位置而非关键字参数的形式。

- 具体用法为使用 / 和 * 符号对参数列表**进行分隔**。

**注：**后面的“\*”语法并不是 Python 3.8 里新增的。

```python
def my_func(a, b=1):
    return a+b
my_func(5,2)     # both positional arguments
my_func(a=5,b=2) # both keyword arguments
```

在下面的例子中，头两个参数 a 和 b 只能用位置参数，中间两个参数 c 和 d 可以任意使用关键字或位置方式指定，最后两个参数 e 和 f 只能用关键字参数。

- `/`和`*`中间的c、d可以是位置参数，也可以是关键字参数

```python
def my_func(a, b, /, c, d, *, e, f):
    return a+b+c+d+e+f
  
my_func(1,2,3,4,5,6)         # invalid as e and f are keyword-only
my_func(a=1,b=2,3,4,e=5,f=6) # invalid as a and b are position-only
my_func(1,2,3,d=4,e=5,f=6)   # returns 21
my_func(1,2,c=3,d=4,e=5,f=6)  # returns 21
```



为什么需要限制这种灵活性呢？如果你的参数名没有什么意义，或者是随便取的（比如 a、b、i、j 这样），那你应该排除使用关键字传递的方式，**免得未来你重构或是修改这个函数的时候，改动参数的变量名称会让其他调用代码出错。**这样就能让你的代码更加的稳定健壮。





# 3 字符串 2.0 版——方便调试

Python 的 f 字符串是一个创举。它使你可以用优雅易懂的方式**格式化输出包含表达式的字符串。**

它的基本语法是 f'{expr}' ，其中需要计算的表达式被大括号括起来，在字符串引号的前面，用字母 f 进行标记。

```python
pi = 3 # I studied Engineering

print(f'π equals {pi}.') # π equals 3.
print(f'{pi=}')          # pi=3


# take it or leave it
```

本次更新给 f 字符串带来了一个**新的格式化标记：**等号“=”。

在 f 字符串里，等号跟在表达式的末尾，语法为：f'{expr=}'，输出的字符串将包含变量名称和其对应的值，如下面这个例子所示：

这样，在调试时，我们就能方便简洁地打出变量的值，而不必写 print('pi =',pi) 这样重复的语句了





# 4 反向迭代字典——顺序

现在 dict 和 dictview 可以使用 reversed()按插入顺序反向迭代。



# 5 新增模块——metadata（元数据）

@模块的metadata

新增的 importlib.metadata 模块使你能够**从第三方包读取元数据。**

例如，你能用代码取得其他包的**版本号**之类的信息。







# 6 在 finally 中使用 Continue

由于在实现中存在问题，之前在 finally 子句中不允许使用 continue 语句。

在 Python 3.8 中这个限制已经被取消了。

```python
for i in range(2):
    try:
        print(i)
    finally:
        print('A sentence.')
        continue
        print('This never shows.')

# Python <= 3.7
>> SyntaxError: 'continue' not supported inside 'finally' clause
  
# Python 3.8
>> 0
   A sentence.
   1
   A sentence.
```



# 总结

注意，本文并未提及Python3.8中新增的一些和普通程序员不太相关的高级特性（比如新的 pickle 协议，以及新的 multiprocessing.shared_memory 模块等）



# #参考文献

[link: 完整的更新说明](https://docs.python.org/zh-cn/3/whatsnew/3.8.html)





