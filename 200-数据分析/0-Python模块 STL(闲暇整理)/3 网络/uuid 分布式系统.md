uuid 是通用唯一识别码 => 理论上，在这个宇宙中不可能生成两个相同的uuid

- 通过MAC地址、时间戳、命名空间、随机数、伪随机数来保证生成ID的唯一性



在==分布式系统==中，你可以用uuid来唯一地表示一个元素，不需要中央控制端来做标识的统一分配，任何一台机器上都可以大胆放心的生成uuid



在很多系统里，uuid都有应用，比如微软的 Microsoft's Globally Unique Identifiers (GUIDs)，Linux ext2/ext3 档案系统。

我在工作中也曾用到过，在==与其他web服务交互==时，使用uuid作为一次请求的唯一标识，在日志中记录下来，这样就可以通过uuid来串联起两个系统的调用链路信息。



uuid的算法有5种：

- uuid3() 和 uuid5() 不常用，工作中使用uuid1()和uuid() 就可以了

| uuid算法 |                                                              |
| -------- | ------------------------------------------------------------ |
| uuid1()  | 基于时间戳, 由MAC地址、当前时间戳、随机数生成,可以保证全球唯一性 |
| uuid2()  | 基于分布式计算环境DCE，python中没有提供这个算法。            |
| uuid3()  | 计算名字和命名空间的MD5散列值，可以保证同一命名空间中不同名字的唯一性，和不同命名空间的唯一性，但同一命名空间的同一名字生成相同的uuid |
| uuid4()  | 由伪随机数得到，有一定的重复概率，该概率可以计算出来         |
| uuid5()  | 基于名字的SHA-1散列值， 算法与uuid3相同，不同的是使用 Secure Hash Algorithm 1 算法 |

```python
import uuid

print(uuid.uuid1())
print(uuid.uuid3(uuid.NAMESPACE_DNS, 'coolpython'))
print(uuid.uuid4())
print(uuid.uuid5(uuid.NAMESPACE_DNS, 'coolpython'))
```