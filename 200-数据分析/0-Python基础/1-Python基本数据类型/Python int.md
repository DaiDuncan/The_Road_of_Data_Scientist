# Python 基本数据类型 | int

**声明**：以下内容主要基于[cnblogs | Python基本数据类型之int](https://www.cnblogs.com/whatisfantasy/p/5956706.html)



## 一 int的范围

### 1 python2.7

- 32位 -231~231-1
- 64位：-263~263-1



### 2 python3.5

int长度理论上是无限的





## 二 python内存机制

在一般情况下当变量被赋值后，内存和变量的关系如下：

### 方式1

```
# 方式一
n1 = 123
n2 = 123
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592983679239-ba0f2da7-a43e-4baf-a076-994188d7ae60.png)

### 方式2

```
#方式二
n1 = 123
n2 = n1
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1136179/1592983700719-ad3c9733-b4e8-4a37-bbec-dcc5c582a2ea.png)

### python优化机制

在-5~257之间的数，如果使用第一种赋值方式，那么他们依然属于同一块内存

```
print(id(n1))    #查看变量的内存地址
```





## 三 进制转换

```
bin()    #把变量转换为2进制
oct()    #把变量转换为8进制
int()    #把变量转换为10进制
hex()    #把变量转换为16进制
```





## 四 源码

```
class int(object):
    """
    int(x=0) -> integer
    int(x, base=10) -> integer
    
    Convert a number or string to an integer, or return 0 if no arguments
    are given.  If x is a number, return x.__int__().  For floating point
    numbers, this truncates towards zero.
    
    If x is not a number or if base is given, then x must be a string,
    bytes, or bytearray instance representing an integer literal in the
    given base.  The literal can be preceded by '+' or '-' and be surrounded
    by whitespace.  The base defaults to 10.  Valid bases are 0 and 2-36.
    Base 0 means to interpret the base from the string as an integer literal.
    >>> int('0b100', base=0)
    4
    """
    def bit_length(self): ; restored from __doc__
        """
        int.bit_length() -> int
        
        Number of bits necessary to represent self in binary.
        >>> bin(37)
        '0b100101'
        >>> (37).bit_length()
        6
        """
        return 0

    def conjugate(self, *args, **kwargs): 
        """ Returns self, the complex conjugate of any int. """
        pass

    @classmethod # known case
    def from_bytes(cls, bytes, byteorder, *args, **kwargs): ; NOTE: unreliably restored from __doc__ 
        """
        int.from_bytes(bytes, byteorder, *, signed=False) -> int
        
        Return the integer represented by the given array of bytes.
        
        The bytes argument must be a bytes-like object (e.g. bytes or bytearray).
        
        The byteorder argument determines the byte order used to represent the
        integer.  If byteorder is 'big', the most significant byte is at the
        beginning of the byte array.  If byteorder is 'little', the most
        significant byte is at the end of the byte array.  To request the native
        byte order of the host system, use `sys.byteorder' as the byte order value.
        
        The signed keyword-only argument indicates whether two's complement is
        used to represent the integer.
        """
        pass

    def to_bytes(self, length, byteorder, *args, **kwargs): ; NOTE: unreliably restored from __doc__ 
        """
        int.to_bytes(length, byteorder, *, signed=False) -> bytes
        
        Return an array of bytes representing an integer.
        
        The integer is represented using length bytes.  An OverflowError is
        raised if the integer is not representable with the given number of
        bytes.
        
        The byteorder argument determines the byte order used to represent the
        integer.  If byteorder is 'big', the most significant byte is at the
        beginning of the byte array.  If byteorder is 'little', the most
        significant byte is at the end of the byte array.  To request the native
        byte order of the host system, use `sys.byteorder' as the byte order value.
        
        The signed keyword-only argument determines whether two's complement is
        used to represent the integer.  If signed is False and a negative integer
        is given, an OverflowError is raised.
        """
        pass

    def __abs__(self, *args, **kwargs): 
        """ abs(self) """
        pass

    def __add__(self, *args, **kwargs): 
        """ Return self+value. """
        pass

    def __and__(self, *args, **kwargs): 
        """ Return self&value. """
        pass

    def __bool__(self, *args, **kwargs): 
        """ self != 0 """
        pass

    def __ceil__(self, *args, **kwargs): 
        """ Ceiling of an Integral returns itself. """
        pass

    def __divmod__(self, *args, **kwargs): 
        """ Return divmod(self, value). """
        pass

    def __eq__(self, *args, **kwargs): 
        """ Return self==value. """
        pass

    def __float__(self, *args, **kwargs): 
        """ float(self) """
        pass

    def __floordiv__(self, *args, **kwargs): 
        """ Return self//value. """
        pass

    def __floor__(self, *args, **kwargs): 
        """ Flooring an Integral returns itself. """
        pass

    def __format__(self, *args, **kwargs): 
        pass

    def __getattribute__(self, *args, **kwargs): 
        """ Return getattr(self, name). """
        pass

    def __getnewargs__(self, *args, **kwargs): 
        pass

    def __ge__(self, *args, **kwargs): 
        """ Return self>=value. """
        pass

    def __gt__(self, *args, **kwargs): 
        """ Return self>value. """
        pass

    def __hash__(self, *args, **kwargs): 
        """ Return hash(self). """
        pass

    def __index__(self, *args, **kwargs): 
        """ Return self converted to an integer, if self is suitable for use as an index into a list. """
        pass

    def __init__(self, x, base=10): # known special case of int.__init__
        """
        int(x=0) -> integer
        int(x, base=10) -> integer
        
        Convert a number or string to an integer, or return 0 if no arguments
        are given.  If x is a number, return x.__int__().  For floating point
        numbers, this truncates towards zero.
        
        If x is not a number or if base is given, then x must be a string,
        bytes, or bytearray instance representing an integer literal in the
        given base.  The literal can be preceded by '+' or '-' and be surrounded
        by whitespace.  The base defaults to 10.  Valid bases are 0 and 2-36.
        Base 0 means to interpret the base from the string as an integer literal.
        >>> int('0b100', base=0)
        4
        # (copied from class doc)
        """
        pass

    def __int__(self, *args, **kwargs): 
        """ int(self) """
        pass

    def __invert__(self, *args, **kwargs): 
        """ ~self """
        pass

    def __le__(self, *args, **kwargs): 
        """ Return self<=value. """
        pass

    def __lshift__(self, *args, **kwargs): 
        """ Return self<<value. """
        pass

    def __lt__(self, *args, **kwargs): 
        """ Return self<value. """
        pass

    def __mod__(self, *args, **kwargs): 
        """ Return self%value. """
        pass

    def __mul__(self, *args, **kwargs): 
        """ Return self*value. """
        pass

    def __neg__(self, *args, **kwargs): 
        """ -self """
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): 
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __ne__(self, *args, **kwargs): 
        """ Return self!=value. """
        pass

    def __or__(self, *args, **kwargs): 
        """ Return self|value. """
        pass

    def __pos__(self, *args, **kwargs): 
        """ +self """
        pass

    def __pow__(self, *args, **kwargs): 
        """ Return pow(self, value, mod). """
        pass

    def __radd__(self, *args, **kwargs): 
        """ Return value+self. """
        pass

    def __rand__(self, *args, **kwargs): 
        """ Return value&self. """
        pass

    def __rdivmod__(self, *args, **kwargs): 
        """ Return divmod(value, self). """
        pass

    def __repr__(self, *args, **kwargs): 
        """ Return repr(self). """
        pass

    def __rfloordiv__(self, *args, **kwargs): 
        """ Return value//self. """
        pass

    def __rlshift__(self, *args, **kwargs): 
        """ Return value<<self. """
        pass

    def __rmod__(self, *args, **kwargs): 
        """ Return value%self. """
        pass

    def __rmul__(self, *args, **kwargs): 
        """ Return value*self. """
        pass

    def __ror__(self, *args, **kwargs): 
        """ Return value|self. """
        pass

    def __round__(self, *args, **kwargs): 
        """
        Rounding an Integral returns itself.
        Rounding with an ndigits argument also returns an integer.
        """
        pass

    def __rpow__(self, *args, **kwargs): 
        """ Return pow(value, self, mod). """
        pass

    def __rrshift__(self, *args, **kwargs): 
        """ Return value>>self. """
        pass

    def __rshift__(self, *args, **kwargs): 
        """ Return self>>value. """
        pass

    def __rsub__(self, *args, **kwargs): 
        """ Return value-self. """
        pass

    def __rtruediv__(self, *args, **kwargs): 
        """ Return value/self. """
        pass

    def __rxor__(self, *args, **kwargs): 
        """ Return value^self. """
        pass

    def __sizeof__(self, *args, **kwargs): 
        """ Returns size in memory, in bytes """
        pass

    def __str__(self, *args, **kwargs): 
        """ Return str(self). """
        pass

    def __sub__(self, *args, **kwargs): 
        """ Return self-value. """
        pass

    def __truediv__(self, *args, **kwargs): 
        """ Return self/value. """
        pass

    def __trunc__(self, *args, **kwargs): 
        """ Truncating an Integral returns itself. """
        pass

    def __xor__(self, *args, **kwargs): 
        """ Return self^value. """
        pass

    denominator = property(lambda self: object(), lambda self, v: None, lambda self: None)  # default
    """the denominator of a rational number in lowest terms"""

    imag = property(lambda self: object(), lambda self, v: None, lambda self: None)  # default
    """the imaginary part of a complex number"""

    numerator = property(lambda self: object(), lambda self, v: None, lambda self: None)  # default
    """the numerator of a rational number in lowest terms"""

    real = property(lambda self: object(), lambda self, v: None, lambda self: None)  # default
    """the real part of a complex number"""
```



## 参考文献

[cnblogs | Python基本数据类型之int](https://www.cnblogs.com/whatisfantasy/p/5956706.html)