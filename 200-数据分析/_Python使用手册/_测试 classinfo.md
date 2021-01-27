```python
print([t for t in __builtins__.__dict__.values() if isinstance(t, type)])
```

```python
[
     <class '_frozen_importlib.BuiltinImporter'>,
     <class 'bool'>,
     <class 'memoryview'>,
     <class 'bytearray'>,
     <class 'bytes'>,
     <class 'classmethod'>,
     <class 'complex'>,
     <class 'dict'>,
     <class 'enumerate'>,
     <class 'filter'>,
     <class 'float'>,
     <class 'frozenset'>,
     <class 'property'>,
     <class 'int'>,
     <class 'list'>,
     <class 'map'>,
     <class 'object'>,
     <class 'range'>,
     <class 'reversed'>,
     <class 'set'>,
     <class 'slice'>,
     <class 'staticmethod'>,
     <class 'str'>,
     <class 'super'>,
     <class 'tuple'>,
     <class 'type'>,
     <class 'zip'>,
     <class 'BaseException'>,
     <class 'Exception'>,
     <class 'TypeError'>,
     <class 'StopAsyncIteration'>,
     <class 'StopIteration'>,
     <class 'GeneratorExit'>,
     <class 'SystemExit'>,
     <class 'KeyboardInterrupt'>,
     <class 'ImportError'>,
     <class 'ModuleNotFoundError'>,
     <class 'OSError'>,
     <class 'OSError'>,
     <class 'OSError'>,
     <class 'OSError'>,
     <class 'EOFError'>,
     <class 'RuntimeError'>,
     <class 'RecursionError'>,
     <class 'NotImplementedError'>,
     <class 'NameError'>,
     <class 'UnboundLocalError'>,
     <class 'AttributeError'>,
     <class 'SyntaxError'>,
     <class 'IndentationError'>,
     <class 'TabError'>,
     <class 'LookupError'>,
     <class 'IndexError'>,
     <class 'KeyError'>,
     <class 'ValueError'>,
     <class 'UnicodeError'>,
     <class 'UnicodeEncodeError'>,
     <class 'UnicodeDecodeError'>,
     <class 'UnicodeTranslateError'>,
     <class 'AssertionError'>,
     <class 'ArithmeticError'>,
     <class 'FloatingPointError'>,
     <class 'OverflowError'>,
     <class 'ZeroDivisionError'>,
     <class 'SystemError'>,
     <class 'ReferenceError'>,
     <class 'BufferError'>,
     <class 'MemoryError'>,
     <class 'Warning'>,
     <class 'UserWarning'>,
     <class 'DeprecationWarning'>,
     <class 'PendingDeprecationWarning'>,
     <class 'SyntaxWarning'>,
     <class 'RuntimeWarning'>,
     <class 'FutureWarning'>,
     <class 'ImportWarning'>,
     <class 'UnicodeWarning'>,
     <class 'BytesWarning'>,
     <class 'ResourceWarning'>,
     <class 'ConnectionError'>,
     <class 'BlockingIOError'>,
     <class 'BrokenPipeError'>,
     <class 'ChildProcessError'>,
     <class 'ConnectionAbortedError'>,
     <class 'ConnectionRefusedError'>,
     <class 'ConnectionResetError'>,
     <class 'FileExistsError'>,
     <class 'FileNotFoundError'>,
     <class 'IsADirectoryError'>,
     <class 'NotADirectoryError'>,
     <class 'InterruptedError'>,
     <class 'PermissionError'>,
     <class 'ProcessLookupError'>,
     <class 'TimeoutError'>
 ]
```



# isinstance(object, **classinfo**)

**classinfo**：也可以是tuple格式

```python
### 针对类
class Foo:
  a = 5
  
fooInstance = Foo()

print(isinstance(fooInstance, Foo))
print(isinstance(fooInstance, (list, tuple)))
print(isinstance(fooInstance, (list, tuple, Foo)))

# Output
True	#是一个Foo类
False
True
```



```python
### 针对内置的数据类型
numbers = [1, 2, 3]

result = isinstance(numbers, list)
print(numbers,'instance of list?', result)

result = isinstance(numbers, dict)
print(numbers,'instance of dict?', result)

result = isinstance(numbers, (dict, list))
print(numbers,'instance of dict or list?', result)

number = 5

result = isinstance(number, list)
print(number,'instance of list?', result)

result = isinstance(number, int)
print(number,'instance of int?', result)


# output
[1, 2, 3] instance of list? True
[1, 2, 3] instance of dict? False
[1, 2, 3] instance of dict or list? True
5 instance of list? False
5 instance of int? True
```

