xml是一种标记语言，被用来设计存储和传输数据，java的项目都喜欢用xml来做配置文件，很多公司的接口服务也喜欢用xml来传输数据



本文重点在于讲解如何读取xml文件，因此关于xml的语法不做讲解



```xml

<?xml version="1.0" encoding="utf-8"?>
<dbconfig>
    <mysql>
        <host>127.0.0.1</host>
        <port>3306</port>
        <dbname>test</dbname>
        <username>root</username>
        <password>4355</password>
    </mysql>
    <redis>
        <host>127.0.0.1</host>
        <port>3306</port>
        <db>0</db>
        <password>44546</password>
    </redis>
</dbconfig>
```



## xml.dom

读取的方法很多，我是用python提供的标准模块

```python
import  xml.dom.minidom

dom = xml.dom.minidom.parse('db.xml')
root = dom.documentElement      # 获得根节点
print(root.nodeName)            # 节点名称 dbconfig

# 获取mysql节点
mysql_node = root.getElementsByTagName('mysql')[0]
for node in mysql_node.childNodes:
    if node.nodeType == 1:   # ELEMENT_NODE
        print(node.nodeName, node.firstChild.data)
        
'''
dbconfig
host 127.0.0.1
port 3306
dbname test
username root
password 4355
'''
```