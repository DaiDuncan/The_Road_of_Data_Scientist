从python3开始，python正式提供了枚举类型。

当一个变量有几种固定的取值时，通常我们喜欢将它定义为枚举类型，枚举类型用于声明一组命名的常数，使用枚举类型可以增强代买的可读性。



枚举类型要求一旦完成定义，就不能再修改，否则使用枚举的地方将由于枚举值的改变出现不可知的问题。



## enum.Enum

enum模块，定义类时继承enum.Enum，可以创建一个枚举类型数据

- 由于继承了enum.Enum，ColorCode的类属性将无法修改

```python
import enum


class ColorCode(enum.Enum):
    RED = 1
    BLUE = 2
    BLACK = 3


def print_color(color_code):
    if color_code == ColorCode.RED.value:
        print('红色')
    elif color_code == ColorCode.BLUE.value:
        print('蓝色')
    elif color_code == ColorCode.BLACK.value:
        print('黑色')

print_color(1)
```





# 枚举值

## unique装饰器

为了防止定义枚举值时出现重复的值，enum模块还提供了unique装饰器

- 如果枚举值出现了重复的情况，由于有unique装饰器修饰，因此在执行时会报错

```python
import enum
from enum import unique

@unique
class ColorCode(enum.Enum):
    RED = 1
    BLUE = 1
    BLACK = 3
```



## 遍历

使用for循环可以对枚举值进行遍历

```python
import enum
from enum import unique

@unique
class ColorCode(enum.Enum):
    RED = 1
    BLUE = 2
    BLACK = 3


for color in ColorCode:
    print(color.name, color.value)
    
    
### Output
RED 1
BLUE 2
BLACK 3
```





## 比较

枚举值之间不支持 > 和 < 操作，但支持==等值比较==和==is身份比较==