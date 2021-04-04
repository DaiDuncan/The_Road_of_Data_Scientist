[link: map用法总结](https://blog.csdn.net/sevenjoin/article/details/81943864)

---

来说一说：使用数组和set来做哈希法的局限。

* 数组的大小是受限制的，而且**如果元素很少，而哈希值太大会造成内存空间的浪费**。
* set是一个集合，**里面放的元素只能是一个key**，而两数之和这道题目，不仅要判断y是否存在而且还要记录y的下表索引位置，因为要返回x 和 y的下表。所以set 也不能用。



map是一种`<key, value>`的结构，本题可以用key保存数值，用value在保存数值所在的下表索引位置。所以使用map最为合适。

C++提供如下三种map：：（详情请看[关于哈希表，你该了解这些！](https://mp.weixin.qq.com/s/g8N6WmoQmsCUw3_BaWxHZA)）

* std::map
* std::multimap
* std::unordered_map 

std::unordered_map 底层实现为哈希，std::map 和std::multimap 的底层实现是红黑树。

同理，==std::map 和std::multimap 的key也是有序==的（这个问题也经常作为面试题，考察对语言容器底层的理解），[哈希表：两数之和](https://mp.weixin.qq.com/s/uVAtjOHSeqymV8FeQbliJQ)中并不需要key有序，选择std::unordered_map 效率更高！