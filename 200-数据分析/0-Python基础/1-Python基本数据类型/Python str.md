# Python 基本数据类型 | str

**声明**：以下内容主要基于[Python基本数据类型之str](https://www.cnblogs.com/whatisfantasy/p/5956747.html)



## 一 创建

```
s = "morra"
s = str("morra")        #str()这种方法会自动找到str类里的_init_方法去执行

----------------------------------------------------

def __init__(self, value='', encoding=None, errors='strict'): 
    """
    str(object='') -> str
    str(bytes_or_buffer[, encoding[, errors]]) -> str
    
    Create a new string object from the given object. If encoding or
    errors is specified, then the object must expose a data buffer
    that will be decoded using the given encoding and error handler.
    Otherwise, returns the result of object.__str__() (if defined)
    or repr(object).
    encoding defaults to sys.getdefaultencoding().
    errors defaults to 'strict'.
    # (copied from class doc)
    """
    pass

----------------------------------------------------
s = str()
s = str("morra")
s = str("morra",encoding='utf-8')
```





## 二 常用功能

### 索引

### 长度

### 切片

### 移除空白 strip()

### 分割 split()

```
## 索引
s="hello"
print(s[0])  #h
print(s[1])  #e
print(s[2])  #l
print(s[3])  #l
print(s[4])  #o
print(s[5])  #报错


## 长度
len(s)


## 切片
s="hello"
print(s[0:2])  #0<=X<2，输出“he”


## 移除空白
strip()


## 分割
partition("字符串"，"分割字符")

# split()
url = "www.google.com/login/ex"

a, b, c = url.split("/")
print(a, b, c)  #www.google.com login ex

x = url.split("/")
print(x)        #['www.google.com', 'login', 'ex']
p = url.split("/", -1)
print(p)        #['www.google.com', 'login', 'ex']

y = url.split("/")[-1]
print(y)        #ex

z = url.split("/", 1)
print(z)        #['www.google.com', 'login/ex']
```





## 三 输出方式

```
## python2.7默认以字节的方式输出
s = "你好"
for i in s:
    print i

OUTPUT:      
�
�
�
�
�
�


## python3.5默认以字符的方式输出
s = "你好"
for i in s:
    print (i)

OUTPUT:        
你
好
```





## 四 源码

```
class str(object):
    """
    str(object='') -> str
    str(bytes_or_buffer[, encoding[, errors]]) -> str
    
    Create a new string object from the given object. If encoding or
    errors is specified, then the object must expose a data buffer
    that will be decoded using the given encoding and error handler.
    Otherwise, returns the result of object.__str__() (if defined)
    or repr(object).
    encoding defaults to sys.getdefaultencoding().
    errors defaults to 'strict'.
    """
    def capitalize(self): 
        """首字母大写"""
        """
        S.capitalize() -> str
        
        Return a capitalized version of S, i.e. make the first character
        have upper case and the rest lower case.
        """
        return ""
    def center(self, width, fillchar=None): 
        """ 内容居中，width：总长度；fillchar：空白处填充内容，默认无 """
        """
        S.center(width[, fillchar]) -> str
        
        Return S centered in a string of length width. Padding is
        done using the specified fill character (default is a space)
        """
        return ""

    def count(self, sub, start=None, end=None): 
        """
        子序列个数
         a = "hello,world"
        ret1 = a.count("o")     
        ret2 = a.count("o",0,4)     #计算"hell"里"o"的个数
        print(ret1)
        print(ret2)
        """
        """
        S.count(sub[, start[, end]]) -> int
        
        Return the number of non-overlapping occurrences of substring sub in
        string S[start:end].  Optional arguments start and end are
        interpreted as in slice notation.
        """
        return 0


    def encode(self, encoding='utf-8', errors='strict'): 
        """编码"""
        """
        S.encode(encoding='utf-8', errors='strict') -> bytes
        
        Encode S using the codec registered for encoding. Default encoding
        is 'utf-8'. errors may be given to set a different error
        handling scheme. Default is 'strict' meaning that encoding errors raise
        a UnicodeEncodeError. Other possible values are 'ignore', 'replace' and
        'xmlcharrefreplace' as well as any other name registered with
        codecs.register_error that can handle UnicodeEncodeErrors.
        """
        return b""

    def endswith(self, suffix, start=None, end=None): 
        """在指定的范围内判断是否以某一个字符结尾"""
        """
        S.endswith(suffix[, start[, end]]) -> bool
        
        Return True if S ends with the specified suffix, False otherwise.
        With optional start, test S beginning at that position.
        With optional end, stop comparing S at that position.
        suffix can also be a tuple of strings to try.
        """
        return False

    def find(self, sub, start=None, end=None): 
        """寻找子序列的位置，如果没找到，返回-1"""
        """
        S.find(sub[, start[, end]]) -> int
        
        Return the lowest index in S where substring sub is found,
        such that sub is contained within S[start:end].  Optional
        arguments start and end are interpreted as in slice notation.
        
        Return -1 on failure.
        """
        return 0

    def format(self, *args, **kwargs): # known special case of str.format
        """
        字符串格式化
        s = "hello {0}，age:{1}"
        new = s.format('Morra',99)
        print(new)
                  """
        """
        S.format(*args, **kwargs) -> str
        
        Return a formatted version of S, using substitutions from args and kwargs.
        The substitutions are identified by braces ('{' and '}').
        """
        pass

    def format_map(self, mapping): 
        """
        S.format_map(mapping) -> str
        
        Return a formatted version of S, using substitutions from mapping.
        The substitutions are identified by braces ('{' and '}').
        """
        return ""

    def index(self, sub, start=None, end=None): 
        """寻找子序列位置，如果没找到，则报错"""
        """
        S.index(sub[, start[, end]]) -> int
        
        Like S.find() but raise ValueError when the substring is not found.
        """
        return 0

    def isalnum(self): 
        """判断是否是字母和数字"""
        """
        S.isalnum() -> bool
        
        Return True if all characters in S are alphanumeric
        and there is at least one character in S, False otherwise.
        """
        return False

    def isalpha(self): 
        """判断是否是字母"""
        """
        S.isalpha() -> bool
        
        Return True if all characters in S are alphabetic
        and there is at least one character in S, False otherwise.
        """
        return False

    def isdecimal(self): 
        """判断是否为小数"""
        """
        S.isdecimal() -> bool
        
        Return True if there are only decimal characters in S,
        False otherwise.
        """
        return False

    def isdigit(self): 
        """判断是否是数字"""
        """
        S.isdigit() -> bool
        
        Return True if all characters in S are digits
        and there is at least one character in S, False otherwise.
        """
        return False

    def isidentifier(self): 
        """
        S.isidentifier() -> bool
        
        Return True if S is a valid identifier according
        to the language definition.
        
        Use keyword.iskeyword() to test for reserved identifiers
        such as "def" and "class".
        """
        return False

    def islower(self): 
        """判断是否存在小写"""
        """
        S.islower() -> bool
        
        Return True if all cased characters in S are lowercase and there is
        at least one cased character in S, False otherwise.
        """
        return False

    def isnumeric(self): 
        """
        S.isnumeric() -> bool
        
        Return True if there are only numeric characters in S,
        False otherwise.
        """
        return False

    def isprintable(self): 
        """
        S.isprintable() -> bool
        
        Return True if all characters in S are considered
        printable in repr() or S is empty, False otherwise.
        """
        return False

    def isspace(self): 
        """判断是否全部为小写"""
        """
        S.isspace() -> bool
        
        Return True if all characters in S are whitespace
        and there is at least one character in S, False otherwise.
        """
        return False

    def istitle(self): 
        """判断是否是标题"""
        """
        S.istitle() -> bool
        
        Return True if S is a titlecased string and there is at least one
        character in S, i.e. upper- and titlecase characters may only
        follow uncased characters and lowercase characters only cased ones.
        Return False otherwise.
        """
        return False

    def isupper(self): 
        """判断是否全部为大写"""
        """
        S.isupper() -> bool
        
        Return True if all cased characters in S are uppercase and there is
        at least one cased character in S, False otherwise.
        """
        return False

    def join(self, iterable): 
        """
        链接方法,可使用可迭代的变量
        li = ['a','b']
        s = "$".join(li)
        print(s)    #a$b
        """
        """
        S.join(iterable) -> str
        
        Return a string which is the concatenation of the strings in the
        iterable.  The separator between elements is S.
        """
        return ""

    def ljust(self, width, fillchar=None): 
        """内容左对齐，右侧填充，与center用法类似"""
        """
        S.ljust(width[, fillchar]) -> str
        
        Return S left-justified in a Unicode string of length width. Padding is
        done using the specified fill character (default is a space).
        """
        return ""

    def lower(self): 
        """使字母小写"""
        """
        S.lower() -> str
        
        Return a copy of the string S converted to lowercase.
        """
        return ""

    def lstrip(self, chars=None): 
        """移除左边空格"""
        """
        S.lstrip([chars]) -> str
        
        Return a copy of the string S with leading whitespace removed.
        If chars is given and not None, remove characters in chars instead.
        """
        return ""

    def maketrans(self, *args, **kwargs): 
        """
        Return a translation table usable for str.translate().
        
        If there is only one argument, it must be a dictionary mapping Unicode
        ordinals (integers) or characters to Unicode ordinals, strings or None.
        Character keys will be then converted to ordinals.
        If there are two arguments, they must be strings of equal length, and
        in the resulting dictionary, each character in x will be mapped to the
        character at the same position in y. If there is a third argument, it
        must be a string, whose characters will be mapped to None in the result.
        """
        pass

    def partition(self, sep): 
        """以sep进行分割，输出元组"""
        """
        S.partition(sep) -> (head, sep, tail)
        
        Search for the separator sep in S, and return the part before it,
        the separator itself, and the part after it.  If the separator is not
        found, return S and two empty strings.
        """
        pass

    def replace(self, old, new, count=None): 
        """
        替换
        s4="hello MORRA hello"
        ret = s4.replace("he","BB")
        ret2 = s4.replace("he","BB",1)  #替换一次
        """
        
        """
        S.replace(old, new[, count]) -> str
        
        Return a copy of S with all occurrences of substring
        old replaced by new.  If the optional argument count is
        given, only the first count occurrences are replaced.
        """
        return ""

    def rfind(self, sub, start=None, end=None): 
        """从右往左找，参见find（）"""
        """
        S.rfind(sub[, start[, end]]) -> int
        
        Return the highest index in S where substring sub is found,
        such that sub is contained within S[start:end].  Optional
        arguments start and end are interpreted as in slice notation.
        
        Return -1 on failure.
        """
        return 0

    def rindex(self, sub, start=None, end=None): 
        """寻找子序列，如果没有则报错"""
        """
        S.rindex(sub[, start[, end]]) -> int
        
        Like S.rfind() but raise ValueError when the substring is not found.
        """
        return 0

    def rjust(self, width, fillchar=None): 
        """右对齐"""
        """
        S.rjust(width[, fillchar]) -> str
        
        Return S right-justified in a string of length width. Padding is
        done using the specified fill character (default is a space).
        """
        return ""

    def rpartition(self, sep): 
        """
        S.rpartition(sep) -> (head, sep, tail)
        
        Search for the separator sep in S, starting at the end of S, and return
        the part before it, the separator itself, and the part after it.  If the
        separator is not found, return two empty strings and S.
        """
        pass


    def rstrip(self, chars=None): 
        """移除右边边空格"""
        """
        S.rstrip([chars]) -> str
        
        Return a copy of the string S with trailing whitespace removed.
        If chars is given and not None, remove characters in chars instead.
        """
        return ""

    def split(self, sep=None, maxsplit=-1): 
        """
    分割字符
        str.split(str="", num=string.count(str))
                  str -- 分隔符，默认为空格
                  num -- 分割次数。    
                  """
        """
        S.split(sep=None, maxsplit=-1) -> list of strings
        
        Return a list of the words in S, using sep as the
        delimiter string.  If maxsplit is given, at most maxsplit
        splits are done. If sep is not specified or is None, any
        whitespace string is a separator and empty strings are
        removed from the result.
        """
        return []

    def splitlines(self, keepends=None): 
        """
        S.splitlines([keepends]) -> list of strings
        
        Return a list of the lines in S, breaking at line boundaries.
        Line breaks are not included in the resulting list unless keepends
        is given and true.
        """
        return []

    def startswith(self, prefix, start=None, end=None): 
        """以XX开始，参见endswith"""
        """
        S.startswith(prefix[, start[, end]]) -> bool
        
        Return True if S starts with the specified prefix, False otherwise.
        With optional start, test S beginning at that position.
        With optional end, stop comparing S at that position.
        prefix can also be a tuple of strings to try.
        """
        return False

    def strip(self, chars=None): 
        """移除两边空格"""
        """
        S.strip([chars]) -> str
        
        Return a copy of the string S with leading and trailing
        whitespace removed.
        If chars is given and not None, remove characters in chars instead.
        """
        return ""

    def swapcase(self): 
        """大小写翻转"""
        """
        S.swapcase() -> str
        
        Return a copy of S with uppercase characters converted to lowercase
        and vice versa.
        """
        return ""

    def title(self): 
        """标题化，即首字母大写"""
        """
        S.title() -> str
        
        Return a titlecased version of S, i.e. words start with title case
        characters, all remaining cased characters have lower case.
        """
        return ""

    def translate(self, table): 
        """
        S.translate(table) -> str
        
        Return a copy of the string S in which each character has been mapped
        through the given translation table. The table must implement
        lookup/indexing via __getitem__, for instance a dictionary or list,
        mapping Unicode ordinals to Unicode ordinals, strings, or None. If
        this operation raises LookupError, the character is left untouched.
        Characters mapped to None are deleted.
        """
        return ""

    def upper(self): 
        """字母大写,在做验证码的时候比较有用"""
        """
        S.upper() -> str
        
        Return a copy of S converted to uppercase.
        """
        return ""

    def zfill(self, width): 
        """
        S.zfill(width) -> str
        
        Pad a numeric string S with zeros on the left, to fill a field
        of the specified width. The string S is never truncated.
        """
        return ""

    def __add__(self, *args, **kwargs): 
        """ Return self+value. """
        pass

    def __contains__(self, *args, **kwargs): 
        """ Return key in self. """
        pass

    def __eq__(self, *args, **kwargs): 
        """ Return self==value. """
        pass

    def __format__(self, format_spec): 
        """
        S.__format__(format_spec) -> str
        
        Return a formatted version of S as described by format_spec.
        """
        return ""

    def __getattribute__(self, *args, **kwargs): 
        """ Return getattr(self, name). """
        pass

    def __getitem__(self, *args, **kwargs): 
        """ Return self[key]. """
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

    def __init__(self, value='', encoding=None, errors='strict'): # known special case of str.__init__
        """
        str(object='') -> str
        str(bytes_or_buffer[, encoding[, errors]]) -> str
        
        Create a new string object from the given object. If encoding or
        errors is specified, then the object must expose a data buffer
        that will be decoded using the given encoding and error handler.
        Otherwise, returns the result of object.__str__() (if defined)
        or repr(object).
        encoding defaults to sys.getdefaultencoding().
        errors defaults to 'strict'.
        # (copied from class doc)
        """
        pass

    def __iter__(self, *args, **kwargs): 
        """ Implement iter(self). """
        pass

    def __len__(self, *args, **kwargs): 
        """ Return len(self). """
        pass

    def __le__(self, *args, **kwargs): 
        """ Return self<=value. """
        pass

    def __lt__(self, *args, **kwargs): 
        """ Return self<value. """
        pass

    def __mod__(self, *args, **kwargs): 
        """ Return self%value. """
        pass

    def __mul__(self, *args, **kwargs): 
        """ Return self*value.n """
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): 
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __ne__(self, *args, **kwargs): 
        """ Return self!=value. """
        pass

    def __repr__(self, *args, **kwargs): 
        """ Return repr(self). """
        pass

    def __rmod__(self, *args, **kwargs): 
        """ Return value%self. """
        pass

    def __rmul__(self, *args, **kwargs): 
        """ Return self*value. """
        pass

    def __sizeof__(self): 
        """ S.__sizeof__() -> size of S in memory, in bytes """
        pass

    def __str__(self, *args, **kwargs): 
        """ Return str(self). """
        pass
```



## 参考文献

[Python基本数据类型之str](https://www.cnblogs.com/whatisfantasy/p/5956747.html)