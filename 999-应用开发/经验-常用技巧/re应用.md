# 提取括号内的内容

- 正则匹配串前加了r就是为了使得里面的特殊符号不用写反斜杠了。

- [ ]具有去特殊符号的作用,也就是说[(]里的(只是平凡的括号

- 正则匹配串里的()是为了提取整个正则串中符合括号里的正则的内容

- .是为了表示除了换行符的任一字符。*克林闭包，出现0次或无限次。

- 加了？是最小匹配，不加是贪婪匹配。

- re.S是为了让.表示除了换行符的任一字符。

```python
import re
 
string = 'abe(ac)ad)'
p1 = re.compile(r'[(](.*?)[)]', re.S)  #最小匹配
p2 = re.compile(r'[(](.*)[)]', re.S)   #贪婪匹配
print(re.findall(p1, string))
print(re.findall(p2, string))


### 输出
['ac']
['ac)ad']
```

