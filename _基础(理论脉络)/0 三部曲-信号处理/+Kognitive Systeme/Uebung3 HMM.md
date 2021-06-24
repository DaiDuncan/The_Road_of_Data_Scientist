[经典讲解视频|贪心科技](https://www.bilibili.com/video/BV1ik4y167kr)

花了一个小时梳理代码



# 对象|单词 => POS tag

## 0 原始数据

- `"data.txt"` => raw_data读取行：格式化 => 小写，变成单词
- `full_corpus`：每一行是一个单词列表
  - 21928 Sätze
  - 包含标点符号
- `full_tagged_corpus`：对应full_corpus的标签列表

```python
with open("data.txt", encoding="utf-8") as corpus_file:
    raw_data = [line.lower().strip().split() for line in corpus_file] #读取行：格式化 => 小写，变成单词
full_corpus = raw_data[::2] #提取一行：是一个单词列表
full_tagged_corpus = raw_data[1::2]
del raw_data	#减少内存空间的消耗

assert len(full_corpus) == len(full_tagged_corpus)	
print(f"{len(full_corpus)} Sätze geladen")	#21928 Sätze geladen
```



## 1 数据划分

+操作技巧|打乱排序

- `test_corpus` => `test_tagged_corpus` 前2000行
- `corpus` => `tagged_corpus`

```python
import random
random.seed(1232)
shuffled_indices = random.sample(range(len(full_corpus)), len(full_corpus))
corpus = [full_corpus[i] for i in shuffled_indices] #打乱顺序
tagged_corpus = [full_tagged_corpus[i] for i in shuffled_indices]

test_corpus, corpus = corpus[:2000], corpus[2000:]
test_tagged_corpus, tagged_corpus = tagged_corpus[:2000], tagged_corpus[2000:]


print(corpus[0])
print(list(zip(corpus[0], tagged_corpus[0])))
'''
['discount', 'rate', ':', '7', '%', '.', 'the', 'charge', 'on', 'loans', 'to', 'depository', 'institutions', 'by', 'the', 'new', 'york', 'federal', 'reserve', 'bank', '.']
[('discount', 'nn'), ('rate', 'nn'), (':', ':'), ('7', 'cd'), ('%', 'nn'), ('.', '.'), ('the', 'dt'), ('charge', 'nn'), ('on', 'in'), ('loans', 'nns'), ('to', 'to'), ('depository', 'nn'), ('institutions', 'nns'), ('by', 'in'), ('the', 'dt'), ('new', 'nnp'), ('york', 'nnp'), ('federal', 'nnp'), ('reserve', 'nnp'), ('bank', 'nnp'), ('.', '.')]
'''
```





## 2 统计|单词的特征之一💡

- `vocabulary=Counter()`
  - counts：统计数量列表(排序)

```python
from collections import Counter #统计值作为单词的表征

vocabulary = Counter()
for sentence in corpus:
    vocabulary.update(sentence)

tag_vocabulary = Counter()
for sentence in tagged_corpus:
    tag_vocabulary.update(sentence)

print(f"Im Text kommen {len(vocabulary)} unterschiedliche Wörter und {len(tag_vocabulary)} unterschiedliche tags vor.")
print("Die 10 häufigsten Wörter sind:")
print(vocabulary.most_common(10)) #Counter()的属性
'''
[(',', 53567), ('the', 53148), ('.', 42888), ('of', 25225), ('to', 24567), ('a', 22034), ('in', 18669), ('and', 18116), ("'s", 10269), ('that', 9383)]
'''
```



```python
import matplotlib.pyplot as plt
import numpy as np
counts = np.array([count for _, count in vocabulary.most_common()])

plt.figure()
plt.yscale("log")
plt.plot(counts)
plt.show()
```

![image-20210604105515752](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604105516.png)

- `cumulative = np.cumsum(counts)` 单词数量
- 分位数：percentile_index => 95%一共有10808单词 => ==重点单词==

```python
cumulative = np.cumsum(counts)
cumulative = cumulative / cumulative[-1]	#所有counts的比例是1(cumulative的最后一个元素就是总和)
percentile_index = np.searchsorted(cumulative, 0.95) #0.95分位数
print(f"{len(np.where(counts == 1)[0])} Wörter kommen nur einmal vor")	#18635 Wörter kommen nur einmal vor
print(f"{percentile_index+1} unterschiedliche Wörter decken 95% des Corpus ab")	#10808
```



## 3 添加首尾标志💡

```python
vocabulary = ["</s>", "<unk>"] + [word for word, count in vocabulary.most_common(percentile_index+1)]
tag_vocabulary = ["EOS"] + [tag for tag, count in tag_vocabulary.most_common()]
```



==构建查询字典：用数字索引代替单词==💡

```python
vocabulary_lookup = {word: i for i, word in enumerate(vocabulary)}	#10810
'''
{'</s>': 0,	#没有添加起始标记
 '<unk>': 1, #没有出现的单词就标记为1 dict.get(word, 1)
 ',': 2,
 'the': 3,
 '.': 4,
 'of': 5,
 'to': 6,
 'a': 7,
 'in': 8,
 'and': 9,
 "'s": 10,
 ……}
'''
tags_lookup = {tag: i for i, tag in enumerate(tag_vocabulary)}	#4
'''
{'EOS': 0,
 'nn': 1,
 'in': 2,
 'nnp': 3,
 'dt': 4,
 'jj': 5,
 'nns': 6,
 ',': 7,
 '.': 8,
 'cd': 9,
 'rb': 10,
 ……}
'''
```



# 处理数据

| 对象               | 含义                           | 维度                                  |
| ------------------ | ------------------------------ | ------------------------------------- |
| start_counts       | freq(Sequenz => 开头特定的Tag) |                                       |
| bigram_counts      | freq(Tag => Tag)               |                                       |
|                    |                                |                                       |
| label_counts       | freq(Wort => 特定的Tag)        | 维度 (vocabulary, tag_vocabulary)     |
|                    |                                |                                       |
| word_start_counts  | freq(Satz => 开头特定的Wort)   | vocabulary数量(含标点)                |
| word_bigram_counts | freq(Wort => Wort)             | 维度 (vocabulary, vocabulary)         |
| tag_start_counts   |                                | tag_vocabulary数量                    |
| tag_bigram_counts  |                                | 维度 (tag_vocabulary, tag_vocabulary) |

- 以上对象的index对应`vocabulary_lookup`的value(相应的key也就是单词Wort)

```python
word_start_counts = np.zeros((len(vocabulary),), dtype=int)
word_bigram_counts = np.zeros((len(vocabulary), len(vocabulary)), dtype=int)
tag_start_counts = np.zeros((len(tag_vocabulary),), dtype=int)
tag_bigram_counts = np.zeros((len(tag_vocabulary), len(tag_vocabulary)), dtype=int)
label_counts = np.zeros((len(vocabulary), len(tag_vocabulary)), dtype=int)

for sentence, tagged in zip(corpus, tagged_corpus): #训练集中的语料
    words = [vocabulary_lookup.get(word, 1) for word in sentence] + [0] #不存在就是索引1，对应<unk>，添加尾部标记 </s>
    tags = [tags_lookup[tag] for tag in tagged] + [0]	#添加尾部标记EOS
    
    if len(words) == 0:
        continue

    word_start_counts[words[0]] += 1 #句子首单词
    tag_start_counts[tags[0]] += 1 #句子首单词的tag
    
    word_pairs = zip(words[:-1], words[1:]) #顺序连接的词对
    for first, second in word_pairs:
        word_bigram_counts[first, second] += 1

    tag_pairs = zip(tags[:-1], tags[1:]) #顺序连接的词的tag对
    for first, second in tag_pairs:
        tag_bigram_counts[first, second] += 1    
    
    for word, tag in zip(words, tags): #词与相应tag
        label_counts[word, tag] += 1
```

```python
label_counts
'''array([[19928,     0,     0, ...,     0,     0,     0],
       [    0,  8736,    31, ...,    36,     9,     2],
       [    0,     1,     1, ...,     1,     1,     1],
       ...,
       [    0,     6,     1, ...,     1,     1,     1],
       [    0,     1,     1, ...,     1,     1,     1],
       [    0,     1,     1, ...,     1,     1,     1]])
'''
```



## 注意|数据稀疏问题💡

没有出现 => 估计概率为0：分母不能为0

- 频率加1：都至少出现1次
  - me|以下代码：必须加1也可以 => 因为最后的结果是概率值，如果是0，那么log(P=0)就是-inf，无法参与后续计算

```python
# 注意：第一个是开始标记
word_start_counts[1:] += 1
word_bigram_counts[1:] += 1
tag_start_counts[1:] += 1
'''
array([   0,  928, 2708, 5697, 4108,  912,  900,    0,    0,  226, 1025,
         13,   55,  737,   62,   34,   99,  363,  263,   14,    9,    0,
         61,    4, 1044,    0,  106,   16,   30,   54,   59,  116,   32,
         35,    0,    0,   59,   67,   10,   13,    5,    0,    0,    7,
         28,   29])
'''
tag_bigram_counts[1:] += 1
label_counts[1:,1:] += 1

word_unigram_counts = label_counts.sum(1) #单词数量 (10810,) 95%的重点单词
'''array([19928, 52370, 53612, ...,    50,    50,    50])'''
tag_unigram_counts = label_counts.sum(0) #标签数量 (46,)
'''
array([ 19928, 156972, 120240, 112807, 101125,  77666,  76850,  64381,
        54193,  51110,  44901,  44502,  39798,  36910,  35378,  34303,
        32910,  29899,  27051,  24410,  21512,  20462,  19971,  18850,
        18682,  18681,  16333,  15540,  14392,  13578,  13375,  13140,
        12958,  12717,  12674,  12397,  12378,  11740,  11282,  11201,
        11040,  10995,  10970,  10909,  10869,  10861])
'''


word_start_probs = word_start_counts / word_start_counts.sum()
word_bigram_probs = word_bigram_counts / word_unigram_counts[:, np.newaxis] #(10810, 10810) / (10810, 1)
word_label_probs = label_counts / word_unigram_counts[:, np.newaxis] #(10810, 46) /(10810, 1)

tag_start_probs = tag_start_counts / tag_start_counts.sum()
tag_bigram_probs = tag_bigram_counts / tag_unigram_counts[:, np.newaxis] #(46, 46) /(46, 1) => (46, 46)
tag_label_probs = label_counts / tag_unigram_counts #(10810, 46)
```



```python
a = np.array([0,1,2])
a[1:] +=1
a	#array([0, 2, 3])
```



# 简单模型: 概率直接代入

## 评价函数|准确率

- .classify()得到标签的索引值 => 统计得到测试语料的tag的准确率

```python
class POSTagger:
    # Abstract base class
    def classify(self, sentence):
        # sentence: 1D np.array von Wortindizes
        # return: Gleich langes 1D array mit Tagindizes
        raise NotImplementedError #假如没有实现：就没有类的多态覆盖 => 执行此处的classify，最终报错
        
def evaluate(tagger): #tagger是一个类：带有classify的方法
    total, correct = 0, 0
    
    #==# 测试集：所有测试集中的语料
    for sentence, tagged in zip(test_corpus, test_tagged_corpus): 
        words = np.array([vocabulary_lookup.get(word, 1) for word in sentence] + [0])
        labels = np.array([tags_lookup[tag] for tag in tagged])

        prediction = tagger.classify(words) #prediction得到标签的索引值

        total += len(labels) #表示一个句子的单词数量
        correct += np.sum(prediction[:-1] == labels)

    print(f"Accuracy: {correct / total:.1%}")
```

- `word_label_probs`：每个单词对应不同标签的概率
  - self.assignments = assignment_probs.argmax(1) => 每个单词最可能的标签tag

- 

```python
class SimpleTagger(POSTagger):
    def __init__(self, assignment_probs):
        self.assignments = assignment_probs.argmax(1) #沿着axis=1(也就是46所对应的列)：挑选出概率最大的值的index => 维度(10810,)
        print(self.assignments) #[0 3 7 ... 1 5 6] 10810单词各自对应的标签 => 训练结果来自word_label_probs中的最大的概率值
        
    
    def classify(self, sentence): # sentence: 1D np.array von Wortindizes
        sequence = self.assignments[sentence] #实参时evaluate函数中的words：也就是每一个句子中的单词的索引 => 得到标签的索引
        return sequence

evaluate(SimpleTagger(word_label_probs)) #(10810, 46)
'''output
[0 3 7 ... 1 5 6]
Accuracy: 88.6%
'''
```



# Decoder: Viterbi|测量序列Wort => 状态估计Tag

- EOS用作起始、结束状态
  - tag_start_probs也就是初始状态的分布概率(原始数据中：首单词tag的概率分布)

```python
transition_probs = np.copy(tag_bigram_probs) #Zustand: Tag的传递概率 (46, 46)
transition_probs[0] = tag_start_probs

emission_probs = tag_label_probs #Zustand: Tag的概率
```

注意：transition是有次序的，所以并不对称

```python
transition_probs[2][1] == transition_probs[1][2] #False 说明不对称
```



注意：$v_t$概率相乘，20次计算后，精读小于double的分辨率 => 使用$log(v_t)$

```python
np.log2(transition_probs[0]) 
'''
array([        -inf,  -4.42622864,  -2.88221875,  -1.80952352,
        -2.28119181,  -4.45129238,  -4.47038013, -14.28576343,
       -14.28576343,  -6.45921494,  -4.28294841, -10.4784085 ,
        -8.4784085 ,  -4.75828642,  -8.3084835 ,  -9.15648041,
        -7.64190724,  -5.77796879,  -6.24136931, -10.37887283,
       -10.96383533, -14.28576343,  -8.33156712, -11.96383533,
        -4.2564762 , -14.28576343,  -7.54429644, -10.19830058,
        -9.33156712,  -8.50440371,  -8.37887283,  -7.41539871,
        -9.24136931,  -9.11583842, -14.28576343, -14.28576343,
        -8.37887283,  -8.19830058, -10.82633181, -10.4784085 ,
       -11.70080092, -14.28576343, -14.28576343, -11.28576343,
        -9.42778243,  -9.37887283])
'''
```



## 代码实现

> - 单引号注释的是原版：参考思路
> - print注释主要用来调试

```python
class HMM(POSTagger):
    def __init__(self, transition, emission):
        self.transition = transition
        self.emission = emission
        self.num_emissions, self.num_states = emission.shape
        assert self.transition.shape == (self.num_states, self.num_states)
        assert self.emission.shape == (self.num_emissions, self.num_states)
        self.log_transition = np.log2(transition)   #(46, 46)
        self.log_emission = np.log2(emission)       #(10810, 46)
        #print(self.log_transition)
        #print(self.log_emission)

    def classify(self, observation):
        '''注意：所有的概率相乘都改为log_prob相加
        INPUT:
            observation: => sentence: 1D np.array von Wortindizes
        OUTPUT:
            sequence: Gleich langes 1D array mit Tagindizes
        '''
        T = len(observation) #t表示序列顺序 => 也就是时刻
        N = self.num_states #46个状态：第一个是初始EOS的转移概率分布
        

        v_log_probs = np.zeros([T, N])
        best_states = np.zeros([T, N], dtype=int) #数字索引是整型数据
        '''
        v_log_probs = [[0]*N for _ in range(T)] 
        best_states = [[0]*N for _ in range(T)]
        '''
        
            
        #step1: init 
        v_log_probs[0] = self.log_transition[0] + self.log_emission[observation[0]]
        best_states[0,:] = 0
        '''
        for i in range(N):
            v_log_probs[0][i] = self.log_transition[0][i] + self.log_emission[observation[0]][i] #???维度匹配
            best_states[0][i] = 0
        '''
        #print(v_log_probs)
        #print(best_states)
        
        
        #step2: iter
        for t in range(1,T):
            for i in range(N):
                temp,maxindex = float('-inf'),0
                for j in range(N):
                    res = v_log_probs[t-1][j] + self.log_transition[j][i]
                    #print(v_log_probs[t-1][j]) #, self.log_transition[j][i]
                    #print(-999, res, temp, "\n")
                    if res > temp:
                        temp = res
                        maxindex = j

                v_log_probs[t][i] = temp + self.log_emission[observation[t]][i]
                best_states[t][i] = maxindex

        #step3: end => 改|直接求下标
        best_state_end = np.argmax(v_log_probs[-1])
        #print("Log-Wahrscheinlichkeit des Viterbi-Pfads ist: ", v_log_probs[-1, best_state_end])
        #print(-1, type(best_state_end))
        #print(999, best_states)
        '''
        p = max(v_log_probs[-1])
        for i in range(N):
            if v_log_probs[-1][i] == p:
                best_state_end = i
        print(-2, best_state_end)
        '''
        
        #step4：backtrack
        sequence = np.zeros(T, dtype=int)
        state = best_state_end
        for t in reversed(range(T-1)):
            #print(0,state)
            state = best_states[t+1][state]
            sequence[t] = state
        sequence[-1] = best_state_end

        return sequence
        
    
    def classify_score(self, observation):
        """vertib-path的概率
        """
        # Ihr code hier
        T = len(observation)
        seq = self.classify(observation)

        v_log_probs = self.log_transition[0][seq[0]] + self.log_emission[observation[0]][seq[0]]
        
        for i in range(T-1):
            v_log_probs += self.log_transition[seq[i]][seq[i+1]] + self.log_emission[observation[i+1]][seq[i+1]]
        return v_log_probs
```



测试：

```python
# Wir haben Nullwahrscheinlichkeiten, die beim Logarithmus Warnungen produzieren
import warnings
warnings.filterwarnings("ignore", "divide by zero") #如果没有这个：显示RuntimeWarning: divide by zero encountered in log2 => 个别数据是0(比如transition[0, 0])

hmm = HMM(transition_probs, #(46, 46)
          emission_probs)   #(10810, 46)

words = np.array([vocabulary_lookup.get(word, 1) for word in corpus[1000]] + [0])

np.testing.assert_equal(hmm.classify(words),
                           np.array([ 3,  3,  7,  9,  6,  5,  7, 11,  5,  1,  2,  4,  1,  2,  6, 13,  5,
                                      6,  2,  9, 14,  9,  8, 17, 20, 12,  2,  4,  1,  8,  0]))
# Log-Wahrscheinlichkeit des Viterbi-Pfads ist -273.094
print(f"Log-Wahrscheinlichkeit des Viterbi-Pfads ist: {hmm.score(words)}")	#-246.27664177066708
```



评价模型的训练结果：

- 模型训练的作用：不同的单词对应不同的tag => 提高对应tag的准确率

```python
%%time 
evaluate(hmm)#or %time evaluate(hmm) 
'''
Accuracy: 91.7%
Wall time: 8min 22s
'''
```



## 参考答案

- inner矩阵是核心 => 运算向量化⭐
  - 基于np.max(, axis=)和np.argmax()进行向量的运算，而不是单个元素比较

```python
	def classify(self, observation):
        length = len(observation)
        # 矩阵：表示路径 => 记录某个时刻的状态的概率 => 从而选取最优的状态 => 动态规划的dp表格
        psi = np.zeros((length, self.num_states), dtype=np.int)
        
        # 表示某个时刻t的所有状态的概率
        delta = np.full((self.num_states,), float("-inf"))
        delta[0] = 0
        
        for t in range(length): #随着时刻t: 一步步进行变化
            #==# inner矩阵是核心
            inner = delta[:, np.newaxis] + self.log_transition #inner是一个矩阵：表示t-1时刻状态到t时刻所有状态的前向概率值 => 在维度0上取max：也就是合并行，保留列的max(保留变换后状态的max概率值)
            delta = np.max(inner, 0) + self.log_emission[observation[t]]
            psi[t] = np.argmax(inner, 0)
        
        best_path = np.empty((length,), dtype=np.int)
        best_path[length-1] = np.argmax(delta)
        for t in reversed(range(1, length)):
            best_path[t-1] = psi[t, best_path[t]]
        return best_path
```





# 补充|Language Modeling

## 评价函数|熵 => PP值

- .score()得到单个句子的log(p) => 整合得到整个语料的熵 =>得到Perplexity

```python
class LanguageModel:
    def score(self, sentence):
        # sentence: 1D np.array von Wortindizes
        # return: Skalar. log-Wahrscheinlichkeit dieses Satzes
        raise NotImplementedError

def evaluate_lm(lm):
    entropy, total = 0, 0
    
    for sentence in test_corpus:
        words = np.array([vocabulary_lookup.get(word, 1) for word in sentence] + [0])

        lprob = lm.score(words)
        if lprob == float("-inf"):
            print(words)

        total += len(words)
        entropy -= lprob

    print(f"Perplexity: {2 ** (entropy / total):.1f}")
```



## 不同模型的比较

### 1 参数

| 语言模型        | 参数个数               |
| --------------- | ---------------------- |
| Unigramm-Modell | V：表示状态个数        |
| HMM             | $T + T^2 +  V \cdot T$ |
| Bigramm-Modell  | $V^2$                  |

### 2 指标Perplexity

- $\hat{H}(W)$是log(p)的值，求和取平均
- me|假如预测完全正确，那么概率就是1，$\hat{H}(W)$也就是0，最后的PP(W)就是1
  - 否则：log(p)小于0，$\hat{H}(W)$也就大于0，那么PP(W)也就变大

![image-20210605151559745](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210605151559.png)

```python
class UnigramLanguageModel(LanguageModel):
    def __init__(self, unigram_probs):
        self.unigram_probs = np.log2(unigram_probs)	#概率值是log
    
    def score(self, sentence):	#针对单个句子
        return self.unigram_probs[sentence].sum()

evaluate_lm(	#针对所有语料
    UnigramLanguageModel(
        word_unigram_counts / word_unigram_counts.sum())
)
'''output
Perplexity: 731.2
'''
```



```python
class BigramLanguageModel(LanguageModel):
    def __init__(self, start_probs, bigram_probs):	#概率值是log
        self.start_probs = np.log2(start_probs)
        self.bigram_probs = np.log2(bigram_probs)
    
    def score(self, sentence):
        lprob = self.start_probs[sentence[0]]	#初始单词
        lprob += self.bigram_probs[sentence[:-1], sentence[1:]].sum()
        return lprob

evaluate_lm(
    BigramLanguageModel(
        word_start_probs,
        word_bigram_probs)
)
'''output
Perplexity: 83.2
'''
```



```python
%%time 
evaluate_lm(hmm)	
'''output
Perplexity: 522.4
Wall time: 4.18 s
'''
```



# Evaluation: Forward-Algorithmus mit Skalierung

1 问题背景

> 1 使用log-概率：概率值多次相乘后很小，可能小于计算机double的分辨率👌
>
> 2 Viterbi可以使用log-概率 VS. 前向算法不可以(为什么：前向算法涉及元素相加求和，一旦转化为log，无法完成此计算 => 被迫转换回原始尺度)
>
> - me|小结：log适合用来计算乘法，但是当其中混含加法时，无法有效计算
>
> 3 不能使用log-概率的挽救办法：归一化skalierung

```python
    def score(self, observation):
        T = len(observation)
        N = self.num_states
        '''log-p的版本
        '''
        log_alpha_t_s = np.log2(np.zeros([T, N]))
        log_alpha_t_s[0][0] = 0 #EOS概率为1 => Log是0

        for t in range(T-1):
            for i in range(N):
                for j in range(N):
                    #问号右边是一个概率值(从状态j变换到状态i) => 需要遍历状态j求和  => 基于log，没有相应的运算
                	log_alpha_t_s[t+1][i] 问号 log_alpha_t_s[t][j] \
                    						+ self.log_transition[j][i] \
                                            + self.log_emission[observation[i], j]
                
        end_probs = 2**(log_alpha_t_s[-1])

        return end_probs.sum()
```

原理细节：
$$
\alpha_t[i] = \sum_j \alpha_{t-1}[j] \cdot transition[j][i]
$$




2 归一化skalierung⭐

![image-20210604121405850](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210604121406.png)

```python
    def score(self, observation):
        T = len(observation)
        
        alpha_t_s = np.zeros([T+1, self.num_states])
        alpha_t_s[0][0] = 1 #EOS概率为1
        c = np.zeros([T+1,])
        c[0] = 1

        for i in range(T-1):
            alpha_t_s[i+1] = np.dot(alpha_t_s[i], self.transition) * self.emission[observation[i]]
            c[i+1] = alpha_t_s[i+1].sum()
            alpha_t_s[i+1] = alpha_t_s[i+1] / c[i+1]
            
        log_prob = np.log2(alpha_t_s[-1][0]) + np.log2(c).sum()

        return 2**log_prob
```



## 参考答案

```python
    def score(self, observation):
        length = len(observation)
        alpha = np.zeros((self.num_states,))
        alpha[0] = 1
        
        c = np.zeros((length,))
        for t in range(length):
            alpha = np.dot(alpha, self.transition) * self.emission[observation[t]]
            c[t] = alpha.sum()
            alpha /= c[t]
        return np.log2(c).sum() + np.log2(alpha[0])
    
    def _score(self, observation):
        length = len(observation)
        alpha = np.zeros((self.num_states,))
        alpha[0] = 1
        for t in range(length):
            alpha = np.dot(alpha, self.transition) * self.emission[observation[t]]
        return np.log2(alpha.sum())
```



# 总结

> 关键在于HMM模型，算法的理解和在什么情境下使用

## 知识点

1 NLP 

- [词性列表](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)



## 通用技巧⭐

1 内存不足 

1）使用少量源数据：95%频次 => 90%

- 训练数据：比如语料使用一部分句子

2）使用Google Colab

3）使用JetBrains Datalore



2 假如有初始状态，涉及状态间的转移，那么==下标统一== => 多出一个0的位置

```python
    def score(self, observation):
        T = len(observation)
        
        alpha_t_s = np.zeros([T+1, self.num_states])
        alpha_t_s[0][0] = 1 #EOS概率为1
        c = np.zeros([T+1,])
        c[0] = 1

        for i in range(T-1):
            alpha_t_s[i+1] = np.dot(alpha_t_s[i], self.transition) * self.emission[observation[i]]
            c[i+1] = alpha_t_s[i+1].sum()
            alpha_t_s[i+1] = alpha_t_s[i+1] / c[i+1]
            
        log_prob = np.log2(alpha_t_s[-1][0]) + np.log2(c).sum()

        return 2**log_prob
```

![image-20210605153435446](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img3/20210605153436.png)



## 常见应用情境

1 zip()

两个相关联的对象组合在一起

- `word_bigram_counts`, `tag_bigram_counts`, `label_counts`

```python
    word_pairs = zip(words[:-1], words[1:])
    for first, second in word_pairs:
        word_bigram_counts[first, second] += 1

    tag_pairs = zip(tags[:-1], tags[1:])
    for first, second in tag_pairs:
        tag_bigram_counts[first, second] += 1    
    
    for word, tag in zip(words, tags):
        label_counts[word, tag] += 1
```



2 numpy创建数组

- `np.zeros(, dtype)`
- `np.full(, float("-inf"))`
- 

```python
def classify(self, observation):
    length = len(observation)
    psi = np.zeros((length, self.num_states), dtype=np.int)
    delta = np.full((self.num_states,), float("-inf"))
    delta[0] = 0

    for t in range(length):
        inner = delta[:, np.newaxis] + self.log_transition
        delta = np.max(inner, 0) + self.log_emission[observation[t]]
        psi[t] = np.argmax(inner, 0)

    best_path = np.empty((length,), dtype=np.int)
    best_path[length-1] = np.argmax(delta)
    for t in reversed(range(1, length)):
        best_path[t-1] = psi[t, best_path[t]]
    return best_path
```

