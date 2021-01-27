# xml.etree

## 简介

XML是实现不同语言或程序之间进行数据交换的协议，==与json类似==：

```python
<data>
    <country name="Liechtenstein">
        <rank updated="yes">2</rank>    #<tag attrib>text</tag>
        <year>2023</year>
        <gdppc>141100</gdppc>   
        <neighbor direction="E" name="Austria" />  #child
        <neighbor direction="W" name="Switzerland" />
    </country>
    <country name="Singapore">
        <rank updated="yes">5</rank>
        <year>2026</year>
        <gdppc>59900</gdppc>
        <neighbor direction="N" name="Malaysia" />
    </country>
    <country name="Panama">
        <rank updated="yes">69</rank>
        <year>2026</year>
        <gdppc>13600</gdppc>
        <neighbor direction="W" name="Costa Rica" />
        <neighbor direction="E" name="Colombia" />
    </country>
</data>
```



## 解析XML

### 1）利用ElementTree.XML将字符串解析成xml对象

```python
from xml.etree import ElementTree as ET

# 打开文件，读取内容
str_xml = open('xo.xml', 'r').read()

# 将字符串解析成xml特殊对象，root代指xml文件的根节点
root = ET.XML(str_xml)      #<Element 'data' at 0x102122278>
```



### 2）利用ElementTree.parse将文件直接解析成xml对象（不需要先open再read，就可以解析xml文件）

```python
from xml.etree import ElementTree as ET

# 直接解析xml文件
tree = ET.parse("xo.xml")

# 获取xml文件的根节点
root = tree.getroot()   #<Element 'data' at 0x102122278>
```



## 操作XML

```python
print(dir(root))        #查看一个节点的所有方法

#1 遍历XML文档的所有内容
from xml.etree import ElementTree as ET
############ 此处省略解析过程 ############

# 顶层标签
print(root.tag)

# 遍历XML文档的第二层
for child in root:
    # 第二层节点的标签名称和标签属性
    print(child.tag, child.attrib)
    # 遍历XML文档的第三层
    for i in child:
        # 第三层节点的标签名称和内容
        print(i.tag,i.text)

=======================================================
#2 遍历XML中指定的节点
from xml.etree import ElementTree as ET
############ 此处省略解析过程 ############

### 操作
# 顶层标签
print(root.tag)


# 遍历XML中所有的year节点
for node in root.iter('year'):
    # 节点的标签名称和内容
    print(node.tag, node.text)
    
=======================================================
#3 修改节点内容
# 由于修改的节点时，均是在内存中进行，其不会影响文件中的内容 \
# 所以，如果想要修改，则需要重新将内存中的内容写到文件
from xml.etree import ElementTree as ET

############ 解析方式一 ############

# 打开文件，读取XML内容
str_xml = open('xo.xml', 'r').read()

# 将字符串解析成xml特殊对象，root代指xml文件的根节点
root = ET.XML(str_xml)

############ 操作 ############

# 顶层标签
print(root.tag)

# 循环所有的year节点
for node in root.iter('year'):
    # 将year节点中的内容自增一
    new_year = int(node.text) + 1
    node.text = str(new_year)

    # 设置属性
    node.set('name', 'alex')
    node.set('age', '18')
    # 删除属性
    del node.attrib['name']

############ 保存文件 ############
tree = ET.ElementTree(root)     #由于解析方式一没有创建tree，因此我们在操作完文件需要保存的时候需要额外创建一个tree
tree.write("newnew.xml", encoding='utf-8')


from xml.etree import ElementTree as ET

############ 解析方式二 ############

# 直接解析xml文件
tree = ET.parse("xo.xml")

# 获取xml文件的根节点
root = tree.getroot()

############ 操作 ############

# 顶层标签
print(root.tag)

# 循环所有的year节点
for node in root.iter('year'):
    # 将year节点中的内容自增一
    new_year = int(node.text) + 1
    node.text = str(new_year)

    # 设置属性
    node.set('name', 'alex')
    node.set('age', '18')
    # 删除属性
    del node.attrib['name']


############ 保存文件 ############
tree.write("newnew.xml", encoding='utf-8')


=======================================================
#4 删除节点
from xml.etree import ElementTree as ET

############ 解析字符串方式打开 ############

# 打开文件，读取XML内容
str_xml = open('xo.xml', 'r').read()

# 将字符串解析成xml特殊对象，root代指xml文件的根节点
root = ET.XML(str_xml)

############ 操作 ############

# 顶层标签
print(root.tag)

# 遍历data下的所有country节点
for country in root.findall('country'):
    # 获取每一个country节点下rank节点的内容
    rank = int(country.find('rank').text)

    if rank > 50:
        # 删除指定country节点
        root.remove(country)

############ 保存文件 ############
tree = ET.ElementTree(root)
tree.write("newnew.xml", encoding='utf-8')


from xml.etree import ElementTree as ET

############ 解析文件方式 ############

# 直接解析xml文件
tree = ET.parse("xo.xml")

# 获取xml文件的根节点
root = tree.getroot()

############ 操作 ############

# 顶层标签
print(root.tag)

# 遍历data下的所有country节点
for country in root.findall('country'):
    # 获取每一个country节点下rank节点的内容
    rank = int(country.find('rank').text)

    if rank > 50:
        # 删除指定country节点
        root.remove(country)

############ 保存文件 ############
tree.write("newnew.xml", encoding='utf-8')

解析文件方式打开，删除，保存
```





## 创建XML文档

### 创建方式1）Element()

```python
from xml.etree import ElementTree as ET

# 创建根节点
root = ET.Element("famliy")


# 创建节点大儿子
son1 = ET.Element('son', {'name': '儿1'})
# 创建小儿子
son2 = ET.Element('son', {"name": '儿2'})

# 在大儿子中创建两个孙子
grandson1 = ET.Element('grandson', {'name': '儿11'})
grandson2 = ET.Element('grandson', {'name': '儿12'})
son1.append(grandson1)
son1.append(grandson2)


# 把儿子添加到根节点中
root.append(son1)
root.append(son2)

tree = ET.ElementTree(root)
tree.write('oooo.xml',encoding='utf-8', short_empty_elements=False)
---------------

#实例归纳：
ele = ET.Element('family',{"age":"18"})     #创建一个根节点
tree = ET.ElementTree(ele)   #通过根节点创建一个tree
tree.write("xx.xml")        #有了tree之后就可以把修改后的数据保存到xx.xml文件中
---------------
```





### 创建方式2）makeelement()

和Element()一样

```python
from xml.etree import ElementTree as ET

# 创建根节点
root = ET.Element("famliy")


# 创建大儿子
# son1 = ET.Element('son', {'name': '儿1'})
son1 = root.makeelement('son', {'name': '儿1'})
# 创建小儿子
# son2 = ET.Element('son', {"name": '儿2'})
son2 = root.makeelement('son', {"name": '儿2'})

# 在大儿子中创建两个孙子
# grandson1 = ET.Element('grandson', {'name': '儿11'})
grandson1 = son1.makeelement('grandson', {'name': '儿11'})
# grandson2 = ET.Element('grandson', {'name': '儿12'})
grandson2 = son1.makeelement('grandson', {'name': '儿12'})

son1.append(grandson1)
son1.append(grandson2)


# 把儿子添加到根节点中
root.append(son1)
root.append(son2)

tree = ET.ElementTree(root)
tree.write('oooo.xml',encoding='utf-8', short_empty_elements=False)
```



### 创建方式3）SubElement

```python
from xml.etree import ElementTree as ET
# 创建根节点
root = ET.Element("famliy")

# 创建节点大儿子
son1 = ET.SubElement(root, "son", attrib={'name': '儿1'})
# 创建小儿子
son2 = ET.SubElement(root, "son", attrib={"name": "儿2"})

# 在大儿子中创建一个孙子
grandson1 = ET.SubElement(son1, "age", attrib={'name': '儿11'})
grandson1.text = '孙子'

et = ET.ElementTree(root)  #生成文档对象
et.write("test.xml", encoding="utf-8", xml_declaration=True, short_empty_elements=False)     #xml_declaration，添加xml注释
```

由于原生保存的XML时默认无缩进，如果想要设置缩进的话， 需要修改保存方式：

```python
from xml.etree import ElementTree as ET
from xml.dom import minidom


def prettify(elem):
    """将节点转换成字符串，并添加缩进。
    """
    rough_string = ET.tostring(elem, 'utf-8')
    reparsed = minidom.parseString(rough_string)
    return reparsed.toprettyxml(indent="\t")

# 创建根节点
root = ET.Element("famliy")


# 创建大儿子
# son1 = ET.Element('son', {'name': '儿1'})
son1 = root.makeelement('son', {'name': '儿1'})
# 创建小儿子
# son2 = ET.Element('son', {"name": '儿2'})
son2 = root.makeelement('son', {"name": '儿2'})

# 在大儿子中创建两个孙子
# grandson1 = ET.Element('grandson', {'name': '儿11'})
grandson1 = son1.makeelement('grandson', {'name': '儿11'})
# grandson2 = ET.Element('grandson', {'name': '儿12'})
grandson2 = son1.makeelement('grandson', {'name': '儿12'})

son1.append(grandson1)
son1.append(grandson2)


# 把儿子添加到根节点中
root.append(son1)
root.append(son1)


raw_str = prettify(root)

f = open("xxxoo.xml",'w',encoding='utf-8')
f.write(raw_str)
f.close()
```





## XML命名空间

```python
from xml.etree import ElementTree as ET

ET.register_namespace('com',"http://www.company.com") #some name

# build a tree structure
root = ET.Element("{http://www.company.com}STUFF")
body = ET.SubElement(root, "{http://www.company.com}MORE_STUFF", attrib={"{http://www.company.com}hhh": "123"})
body.text = "STUFF EVERYWHERE!"

# wrap it in an ElementTree instance, and save as XML
tree = ET.ElementTree(root)

tree.write("page.xml",
           xml_declaration=True,
           encoding='utf-8',
           method="xml")
```

