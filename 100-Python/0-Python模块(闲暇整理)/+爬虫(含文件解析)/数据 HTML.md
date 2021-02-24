## Python实现HTML语法

HTML：

```html
<ul>
<li>first string</li>
<li>second string</li>
</ul>
```



```python
items = ['first string', 'second string']
html_str = "<ul>\n"

for item in items:
    html_str +="<li>{}</li>\n",format(item)
html_str += "</ol>"

print(html_str)
```



# 解析HTML

| 目标                                                         |                                             |
| ------------------------------------------------------------ | ------------------------------------------- |
| 基于Beautiful Soup，python爬取数据  +提取生成HTML文件  +最终将网页解析为CSV文件 | https://www.youtube.com/watch?v=XQgXKtPSzUI |
| 基于lxml，python 提取HTML数据                                | https://www.youtube.com/watch?v=5N066ISH8og |



