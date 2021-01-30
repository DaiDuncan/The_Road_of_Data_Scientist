对于进阶者来说，你的注意力应当从关注语言基础语法和特性逐渐向==软件工程==方面进行转移

black是一款非常霸道的代码格式化工具，当它检测到不合格的代码时，会直接帮你修改，甚至不询问你的意见



```python
a=4
def test():
    print("test")
```

直观的看，没有任何语法问题，但是它并不符合PEP8规范

pycharm会自动检测代码格式并给出修改建议，然而仅仅是建议，你可以不去理会它

```python
pip3 install black


### 使用时，只需要在black命令后面  指定需要格式化的文件或者目录
~/kwsy/coolpython$  black demo.py
reformatted demo.py
All done! ✨ 🍰 ✨
1 file reformatted

# 格式化后
a = 4


def test():
    print("test")
```

相比于之前的代码，有两处改变:

1. 赋值语句的等号两侧各增加一个空格
2. 函数定义前与上一条语句之间保留两个空行



