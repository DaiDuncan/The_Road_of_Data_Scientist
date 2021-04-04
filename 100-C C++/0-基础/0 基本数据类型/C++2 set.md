[link: CSDN|set用法总结](https://blog.csdn.net/sevenjoin/article/details/81908754)

---

关于set，C++ 提供了如下三种可用的数据结构：（详情请看[关于哈希表，你该了解这些！](https://mp.weixin.qq.com/s/g8N6WmoQmsCUw3_BaWxHZA)）

* std::set
* std::multiset
* std::unordered_set

std::set和std::multiset底层实现都是红黑树，std::unordered_set的底层实现是哈希， 使用unordered_set 读写效率是最高的，**本题并不需要对数据进行排序，而且还不要让数据重复**，所以选择unordered_set。