本文内容源自[medium文章](https://medium.com/analytics-vidhya/a-knowledge-graph-implementation-tutorial-for-beginners-3c53e8802377)

> A Knowledge Graph understanding and implementation tutorial for beginners[1]



# 什么是知识图谱？

知识图谱的内容通常以**三元组形式**存在，**Subject-Predicate-Object** (spo) 主-谓-宾

举个栗子：

> *Leonard Nimoy was an actor who played the character Spock in the science-fiction movie Star Trek*.

对上面的句子可以抽取到如下三元组：

![image-20200518115208499](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518115208499.png)

以知识图谱形式可以表示为：

![image-20200518115310348](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518115310348.png)

上述由节点和关系组成的图，就是一个简单的知识图谱。



# 如何搭建一个简单的知识图谱？

可以分为以下两大步骤：

- 知识提取
  - 信息抽取，==获取三元组==
  - 实体识别、实体链接、实体消歧(Disambiguation)、实体统一(Entity Resolution)
- 图构建
  - 存储
  - 查询

知识提取步骤是构建知识图谱的关键，三元组可以通过**依存分析**得到。



## **动手构建一个简单知识图谱**

此处只显示代码执行过程与结果，完整代码请见[github](https://github.com/Aida-yy/Knowledge-graph-tutorial).

**1. 三元组提取**

借助spacy

```python
inputText = 'Startup companies create jobs and innovation. Bill Gates supports entrepreneurship.'

# Step 1: Knowledge Extraction. 
# Output: SOP triples
knowledgeExtractionObj = KnowledgeExtraction()
sop_list = knowledgeExtractionObj.retrieveKnowledge(inputText)
#list_sop = sop_list.as_doc()
sop_list_strings = []
for sop in sop_list:
    temp = []
    temp.append(sop[0].text)
    temp.append(sop[1].text)
    temp.append(sop[2].text)
    sop_list_strings.append(temp)

print(sop_list_strings)
```

结果

![image-20200518121130941](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518121130941.png)



**2. 实体链接**

```python
# Step 2: Entity recognition and linking. This step needs to be linked.
entityRecognitionLinkingObj = EntityRecognitionLinking()
entityRelJson = entityRecognitionLinkingObj.entityRecogLink(inputText)

entityLinkTriples = []
for sop in sop_list_strings:
    tempTriple = ['', '', '']
    for resource in entityRelJson['Resources']:
        if resource['@surfaceForm'] == sop[0]:
            tempTriple[0] = resource['@URI']
        if resource['@surfaceForm'] == sop[1]:
            tempTriple[1] = resource['@URI']
        if resource['@surfaceForm'] == sop[2]:
            tempTriple[2] = resource['@URI']
    entityLinkTriples.append(tempTriple)
print(entityLinkTriples)
```

结果

![image-20200518121205037](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518121205037.png)



**3. 图构建**

使用neo4j

```python
# Step 3: Knowledge Graph creation.
graphPopulationObj = GraphPopulation()
graphPopulationObj = graphPopulationObj.popGraph(
    sop_list_strings, entityLinkTriples)
```

![image-20200518121223303](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518121223303.png)

最终得到图如下：

![image-20200518121314890](https://gitee.com/gongyanzh/blogpic/raw/master/pictures/image-20200518121314890.png)

## **可能遇到的问题**

### Q1

```
AuthError: The client is unauthorized due to authentication failure.
```

解决办法：确保图数据库配置时密码一致与设置的一致 (以下配置表示，user：neo4j，password：neo4j)

```
config.DATABASE_URL = 'bolt://neo4j:neo4j@localhost:7687'#default
```



### Q2

```
ServiceUnavailable: Failed to establish connection to ('127.0.0.1', 7687) (reason [WinError 10061] 由于目标计算机积极拒绝，无法连接。)
```

解决办法：

确保在执行图创建代码前已经打开neo4j





# #参考文献

[1]https://medium.com/analytics-vidhya/a-knowledge-graph-implementation-tutorial-for-beginners-3c53e8802377

[2]https://github.com/kramankishore/Knowledge-Graph-Intro

[3]https://neomodel.readthedocs.io/en/latest/getting_started.html#connecting

[4]https://www.analyticsvidhya.com/blog/2019/10/how-to-build-knowledge-graph-text-using-spacy/