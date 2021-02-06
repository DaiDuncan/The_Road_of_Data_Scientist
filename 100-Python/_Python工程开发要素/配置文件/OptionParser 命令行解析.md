OptionParser 是python内置的功能非常强大的命令行参数解析模块

它使用灵活，可以方便的生成标准的、符合Unix/Posix 规范的命令行说明



```python
import sys
from optparse import OptionParser
from optparse import IndentedHelpFormatter


class NoWrapFormatter(IndentedHelpFormatter) :
    def _format_text(self, text) :
        "[Does not] format a text, return the text as it is."
        return text

parser = OptionParser(prog='coolpython',
                              usage="%prog [OPTION]... FILE...",
                              description="酷python，分享最专业的python技术",
                              version="1.1",
                              formatter=NoWrapFormatter(),
                              epilog="""\
这一段只是为了演示效果
                              """)
parser.add_option("-n", "--name", action="store", help="姓名")

parser.add_option("-a", "--age", action="store", help="年龄")

parser.add_option('-o', "--out", action="store_true", help="是否输出")


if __name__ == '__main__':
    (options, args) = parser.parse_args(sys.argv[1:])
    print((options, args))
    print(options.age)
```





执行脚本 python demo.py -h

```text
Usage: coolpython [OPTION]... FILE...

酷python，分享最专业的python技术

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -n NAME, --name=NAME  姓名
  -a AGE, --age=AGE     年龄
  -o, --out             是否输出

这一段只是为了演示效果
```

OptionParser 会自动生成命令行的说明，这一点是非常棒的

毕竟运行程序需要哪些参数，==这些参数的解释和作用==只有写程序的人才知道

通过自动生成的命令行参数解释说明，运行程序的人就可以正确的使用它。





## 添加参数

使用add_option就可以很方便的添加程序运行可选项说明：

- 放在最前面的是可选项的名称，连续两个--的是可选项的完成名称，一个 - 的是可选项的别名，通常是可选项名称的首字母，便于输入
- action选择store，则将可选项名称作为属性存储，存储的值正是执行程序时所设置的数值
- action选择store_true 或者store_false，则表示将可选项做为属性或存储为True，或存储为False
  - 如果执行程序时没有设置这个可选项，则这个属性为None。



以如下方式执行脚本：

```shell
python demo.py -n 小明 -a 14 -o
(<Values at 0x173e51e2048: {'name': '小明', 'age': '14', 'out': True}>, [])
14
```

parse_args方法的返回值有两个，分别是options 和 args，由于我在执行脚本时除了可选项之外没有提供其他的输入，因此args是一个空列表，你可以试试python demo.py -n 小明 -a 14 -o 90 49 来观察args的变化。

- options是一个类似字典的对象，通过属性名称可以直接访问到参数值。





之所以称它为命令行可选项解析模块，是因为这些参数都是可选项

执行脚本时，你可以选择其中的某几个进行设置，如果某个可选项需要数值，那么可以在add_option方法里添加default参数。

除了可以指定默认值，还可以指定类型，type参数可以设置可选项数值的类型

```python
parser.add_option("-a", "--age", action="store", help="年龄", default=20, type='int')
```

如此一来，解析后的age参数就是int类型的，type支持的类型有 "string", "int", "long", "float", "complex", "choice"

