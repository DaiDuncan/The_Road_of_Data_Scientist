## map|映射

map/dict

- key
- value

一组keys被称为set



## Hashing

哈希函数：const时间 => 类似数组的index



### 哈希方法

尾数取余作为哈希值



### 哈希冲突

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151540.png" alt="〇 ル ち レ - → % び  G.  し 5 勤 し  - → 、 し % レ し / " style="zoom:33%;" />

1 改变哈希函数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151550.png" alt="img" style="zoom:67%;" />

2 3 扩大哈希表 & 集合：列表

- Normal Array：占用空间

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151606.png" alt="し 。 十 貧 つ  つ O ℃  ド ノ 。 N " style="zoom:50%;" />

- Bucket(桶)：消耗时间

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151621.png" alt="1010 \ 罒 " style="zoom: 50%;" />

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151845.png" alt="Bucket Array  value  123  myString  3  bucket_in dex  A node  A bucket  HashMap " style="zoom:50%;" />

@哈希冲突

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151859.png" alt="Bucket Array  Each non-empty 'bucket at position s bucket _ index is  actually a chain (LinkedList) with I or more nodes.  21  bucket index  strl  Key  123  HashMap  str2  Key  456  str3  789  bucket_index generated by three different keys is same " style="zoom:50%;" />



4 嵌套哈希

前提：了解数据结构——>构造合理的哈希函数

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210325151711.png" alt="img" style="zoom: 50%;" />

# Python|dict





