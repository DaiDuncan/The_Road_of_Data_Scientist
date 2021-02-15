## 引言

在Web应用发展的初期，关系型数据库受到了较为广泛的关注和应用，原因是因为那时候Web站点基本上访问和==并发不高、交互也较少==。

而在后来，随着访问量的提升，使用关系型数据库的Web站点多多少少都开始在性能上出现了一些瓶颈，而瓶颈的源头一般是在==磁盘的I/O==上。

而随着互联网技术的进一步发展，各种类型的应用层出不穷，这导致在当今==云计算、大数据盛行==的时代，对性能有了更多的需求，主要体现在以下四个方面：

1. 低延迟的读写速度：应用快速地反应能极大地提升用户的满意度
2. 支撑海量的数据和流量：对于搜索这样大型应用而言，需要利用PB级别的数据和能应对百万级的流量
3. 大规模集群的管理：系统管理员希望==分布式==应用能更简单的部署和管理
4. 庞大运营成本的考量：IT部门希望在硬件成本、软件成本和人力成本能够有大幅度地降低

为了克服这一问题，NoSQL应运而生，它同时具备了高性能、可扩展性强、高可用等优点，受到广泛开发人员和仓库管理人员的青睐。



# what|Redis是什么？

Redis是现在最受欢迎的NoSQL数据库之一

Redis是一个使用ANSI C编写的==开源==、包含多种数据结构、支持网络、基于内存、可选持久性的==键值对存储数据库==，其具备如下特性：

- 基于内存运行，性能高效
- ==支持分布式==，理论上可以无限扩展
- key-value存储系统
- 开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API



相比于其他数据库类型，Redis具备的特点是：

- C/S通讯模型
- ==单进程单线程模型==
- 丰富的数据类型
- 操作具有原子性
- 持久化
- ==高并发读写==
- 支持lua脚本



## Redis的数据类型

Redis提供的数据类型主要分为5种自有类型和一种自定义类型

这5种自有类型包括：String类型、哈希类型、列表类型、集合类型和顺序集合类型。

### String类型

它是一个二进制安全的字符串，意味着它不仅能够存储字符串、还能存储图片、视频等多种类型, 最大长度支持512M。

对每种数据类型，Redis都提供了丰富的操作命令，如：

- GET/MGET
- SET/SETEX/MSET/MSETNX
- INCR/DECR
- GETSET
- DEL



### 哈希类型

该类型是由field和关联的value组成的map。其中，field和value都是字符串类型的。

Hash的操作命令如下：

- HGET/HMGET/HGETALL
- HSET/HMSET/HSETNX
- HEXISTS/HLEN
- HKEYS/HDEL
- HVALS



### 列表类型

该类型是一个插入顺序排序的字符串元素集合, 基于双链表实现。

List的操作命令如下：

- LPUSH/LPUSHX/LPOP/RPUSH/RPUSHX/RPOP/LINSERT/LSET
- LINDEX/LRANGE
- LLEN/LTRIM



### 集合类型

Set类型是一种无顺序集合, 它和List类型最大的区别是：集合中的元素没有顺序, 且元素是唯一的。

Set类型的底层是通过哈希表实现的，其操作命令为：

- SADD/SPOP/SMOVE/SCARD
- SINTER/SDIFF/SDIFFSTORE/SUNION

Set类型主要应用于：在某些场景，如社交场景中，通过交集、并集和差集运算，通过Set类型可以非常方便地查找==共同好友、共同关注和共同偏好等社交关系==。



### 顺序集合类型

ZSet是一种有序集合类型，每个元素都会关联一个double类型的分数权值，通过这个权值来为集合中的成员进行从小到大的排序。与Set类型一样，其底层也是通过哈希表实现的。

ZSet命令：

- ZADD/ZPOP/ZMOVE/ZCARD/ZCOUNT
- ZINTER/ZDIFF/ZDIFFSTORE/ZUNION





## Redis的数据结构

![image-20210203162145458](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162145.png)

1. 压缩列表是列表键和哈希键的底层实现之一。

   当一个列表键只包含少量列表项，并且每个列表项要么就是小整数，要么就是长度比较短的字符串，Redis就会使用压缩列表来做列表键的底层实现

2. 整数集合是集合键的底层实现之一

   当一个集合只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现



定义一个Struct数据结构的例子：

![image-20210203162341205](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162341.png)



### SDS|简单动态字符串SDS (Simple Dynamic String)

基于C语言中传统字符串的缺陷，Redis自己构建了一种名为简单动态字符串的抽象类型，简称SDS，其结构如下：

![image-20210203162539925](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162540.png)



SDS几乎贯穿了Redis的所有数据结构，应用十分广泛。

和C字符串相比，SDS的特点如下：

![image-20210203162609269](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162609.png)



1 常数复杂度获取字符串长度

Redis中利用SDS字符串的len属性可以直接获取到所保存的字符串的长度，直接将获取字符串长度所需的复杂度从C字符串的O(N)降低到了O(1)



2 减少修改字符串时导致的内存重新分配次数

通过C字符串的特性，我们知道对于一个包含了N个字符的C字符串来说，其底层实现总是N+1个字符长的数组（额外一个空字符结尾）

那么如果这个时候需要对字符串进行修改，程序就需要提前对这个C字符串数组进行一次内存重分配（可能是扩展或者释放）　而内存重分配就意味着是一个耗时的操作。



与此同时，SDS采用了**空间预分配**的策略，避免C字符串每一次修改时都需要进行内存重分配的耗时操作，将内存重分配从原来的每修改N次就分配N次——>降低到了修改N次最多分配N次。





## Redis主要特性

### 1 事务

- 命令序列化，按顺序执行
- 原子性
- 三阶段: 开始事务 - 命令入队 - 执行事务
- 命令：MULTI/EXEC/DISCARD



### 2 发布订阅(Pub/Sub)

- Pub/sub是一种消息通讯模式
- Pub发送消息, Sub接受消息
- Redis客户端可以订阅任意数量的频道
- “fire and forgot”, 发送即遗忘
- 命令：Publish/Subscribe/Psubscribe/UnSub

![image-20210203162858917](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162859.png)

### 3 Stream

- Redis 5.0新增
- 等待消费
- 消费组(组内竞争)
- 消费历史数据
- FIFO

![image-20210203162917709](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162917.png)











# how|Redis的应用场景有哪些？

哪些大厂在使用Redis？

- github
- twitter
- 微博
- Stack Overflow
- 阿里巴巴
- 百度
- 美团
- 搜狐



Redis 的应用场景包括：

- 缓存系统（“热点”数据：高频读、低频写）
- 计数器
- ==消息队列系统==
- 排行榜
- 社交网络
- 实时系统



# Redis常见问题

## 1 击穿

概念：在Redis获取某一key时, 由于key不存在, 而必须向DB发起一次请求的行为, 称为“Redis击穿”。

![image-20210203162951787](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210203162951.png)

引发击穿的原因：

- 第一次访问
- 恶意访问不存在的key
- Key过期



合理的规避方案：

- 服务器启动时, 提前写入
- 规范key的命名, 通过中间件拦截
- ==对某些高频访问的Key，设置合理的TTL或永不过期==



## 雪崩

概念：Redis缓存层由于某种原因宕机后，==所有的请求会涌向存储层==，短时间内的==高并发请求==可能会导致存储层挂机，称之为“Redis雪崩”。

合理的规避方案：

- 使用Redis集群
- 限流



# Redis在产品开发中的应用实践

- 后端采用nodeJS
- 使用Azure的Redis服务
- Redis的使用场景
  - token缓存, 用于令牌验证
  - IP白名单



碰到的问题

- “网络抖动”或者Redis服务异常导致Redis访问超时
- Redis客户端驱动稳定性问题
  - 连接池 “Broken connection” 问题
  - JS的Promise引出的Redis重置问题



# 进阶|Redis协议

Redis客户端通讯协议：RESP(Redis Serialization Protocol)，其特点是：

- 简单
- 解析速度快
- 可读性好



Redis集群内部通讯协议：RECP(Redis Cluster Protocol ) ，其特点是：

- 每一个node两个tcp 连接
- 一个负责client-server通讯(P: 6379)
- 一个负责node之间通讯(P: 10000 + 6379)





# 补充|其他NoSQL型数据库

除了Redis，还有MemCache、Cassadra和Mongo

**Memcache**

这是一个和Redis非常相似的数据库，但是它的==数据类型没有Redis丰富==。

Memcache由LiveJournal的Brad Fitzpatrick开发，作为一套==分布式==的高速缓存系统，被许多网站使用以提升网站的访问速度，对于一些大型的、需要频繁访问数据库的网站访问速度的提升效果十分显著。



**Apache Cassandra**

（社区内一般简称为C*）这是一套==开源分布式==NoSQL数据库系统。

它最初由Facebook开发，用于储存收件箱等简单格式数据，集Google BigTable的数据模型与Amazon Dynamo的完全分布式架构于一身。

Facebook于2008将 Cassandra 开源，由于其良好的可扩展性和性能，被 Apple、Comcast、Instagram、Spotify、eBay、Rackspace、Netflix等知名网站所采用，成为了一种流行的分布式结构化数据存储方案。



**MongoDB**

是一个==基于分布式文件存储、面向文档==的NoSQL数据库，由C++编写，旨在为WEB应用提供可扩展的高性能数据存储解决方案。

MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系型数据库的，它==支持的数据结构非常松散，是一种类似json的BSON格式==。



# #补充|redis速查表

[Link: 工具使用|速查表](http://www.redis-doc.com/)

```c
// 追加一个值到key上

APPEND key value
// 验证服务器命令

AUTH password
// 异步重写追加文件命令

BGREWRITEAOF
// 异步保存数据集到磁盘上

BGSAVE
// 统计字符串指定起始位置的字节数

BITCOUNT key [start end]
// Perform arbitrary bitfield integer operations on strings

BITFIELD key [GET type offset] [SET type offset value] [INCRBY type offset increment] [OVERFLOW WRAP|SAT|FAIL]
// Perform bitwise operations between strings

BITOP operation destkey key [key ...]
// Find first bit set or clear in a string

BITPOS key bit [start] [end]
// 删除，并获得该列表中的第一元素，或阻塞，直到有一个可用

BLPOP key [key ...] timeout
// 删除，并获得该列表中的最后一个元素，或阻塞，直到有一个可用

BRPOP key [key ...] timeout
// 弹出一个列表的值，将它推到另一个列表，并返回它;或阻塞，直到有一个可用

BRPOPLPUSH source destination timeout
// Remove and return the member with the highest score from one or more sorted sets, or block until one is available

BZPOPMAX key [key ...] timeout
// Remove and return the member with the lowest score from one or more sorted sets, or block until one is available

BZPOPMIN key [key ...] timeout
// 关闭客户端连接

CLIENT KILL [ip:port] [ID client-id] [normal|slave|pubsub] [ADDR ip:port] [SKIPME yes/no]
// 获得客户端连接列表

CLIENT LIST
// 获得当前连接名称

CLIENT GETNAME
// Returns the client ID for the current connection

CLIENT ID
// 暂停处理客户端命令

CLIENT PAUSE timeout
// Instruct the server whether to reply to commands

CLIENT REPLY ON|OFF|SKIP
// 设置当前连接的名字

CLIENT SETNAME connection-name
// Unblock a client blocked in a blocking command from a different connection

CLIENT UNBLOCK client-id [TIMEOUT|ERROR]
// Assign new hash slots to receiving node

CLUSTER ADDSLOTS slot [slot ...]
// Return the number of failure reports active for a given node

CLUSTER COUNT-FAILURE-REPORTS node-id
// Return the number of local keys in the specified hash slot

CLUSTER COUNTKEYSINSLOT slot
// Set hash slots as unbound in receiving node

CLUSTER DELSLOTS slot [slot ...]
// Forces a slave to perform a manual failover of its master.

CLUSTER FAILOVER [FORCE|TAKEOVER]
// Remove a node from the nodes table

CLUSTER FORGET node-id
// Return local key names in the specified hash slot

CLUSTER GETKEYSINSLOT slot count
// Provides info about Redis Cluster node state

CLUSTER INFO
// Returns the hash slot of the specified key

CLUSTER KEYSLOT key
// Force a node cluster to handshake with another node

CLUSTER MEET ip port
// Get Cluster config for the node

CLUSTER NODES
// List replica nodes of the specified master node

CLUSTER REPLICAS node-id
// Reconfigure a node as a slave of the specified master node

CLUSTER REPLICATE node-id
// Reset a Redis Cluster node

CLUSTER RESET [HARD|SOFT]
// Forces the node to save cluster state on disk

CLUSTER SAVECONFIG
// Set the configuration epoch in a new node

CLUSTER SET-CONFIG-EPOCH config-epoch
// Bind an hash slot to a specific node

CLUSTER SETSLOT slot IMPORTING|MIGRATING|STABLE|NODE [node-id]
// List slave nodes of the specified master node

CLUSTER SLAVES node-id
// Get array of Cluster slot to node mappings

CLUSTER SLOTS
// Get array of Redis command details

COMMAND
// Get total number of Redis commands

COMMAND COUNT
// Extract keys given a full Redis command

COMMAND GETKEYS
// Get array of specific Redis command details

COMMAND INFO command-name [command-name ...]
// 获取配置参数的值

CONFIG GET parameter
// 从写内存中的配置文件

CONFIG REWRITE
// 设置配置文件

CONFIG SET parameter value
// 复位再分配使用info命令报告的统计

CONFIG RESETSTAT
// 返回当前数据库里面的keys数量

DBSIZE
// 获取一个key的debug信息

DEBUG OBJECT key
// 使服务器崩溃命令

DEBUG SEGFAULT
// 整数原子减1

DECR key
// 原子减指定的整数

DECRBY key decrement
// 删除指定的key（一个或多个）

DEL key [key ...]
// 丢弃所有 MULTI 之后发的命令

DISCARD
// 导出key的值

DUMP key
// 回显输入的字符串

ECHO message
// 在服务器端执行 LUA 脚本

EVAL script numkeys key [key ...] arg [arg ...]
// 在服务器端执行 LUA 脚本

EVALSHA sha1 numkeys key [key ...] arg [arg ...]
// 执行所有 MULTI 之后发的命令

EXEC
// 查询一个key是否存在

EXISTS key [key ...]
// 设置一个key的过期的秒数

EXPIRE key seconds
// 设置一个UNIX时间戳的过期时间

EXPIREAT key timestamp
// 清空所有数据库命令

FLUSHALL
// 清空当前的数据库命令

FLUSHDB
// 添加一个或多个地理空间位置到sorted set

GEOADD key longitude latitude member [longitude latitude member ...]
// 返回一个标准的地理空间的Geohash字符串

GEOHASH key member [member ...]
// 返回地理空间的经纬度

GEOPOS key member [member ...]
// 返回两个地理空间之间的距离

GEODIST key member1 member2 [unit]
// 查询指定半径内所有的地理空间元素的集合。

GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]
// 查询指定半径内匹配到的最大距离的一个地理空间元素。

GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]
// 返回key的value

GET key
// 返回位的值存储在关键的字符串值的偏移量。

GETBIT key offset
// 获取存储在key上的值的一个子字符串

GETRANGE key start end
// 设置一个key的value，并获取设置前的值

GETSET key value
// 删除一个或多个Hash的field

HDEL key field [field ...]
// 判断field是否存在于hash中

HEXISTS key field
// 获取hash中field的值

HGET key field
// 从hash中读取全部的域和值

HGETALL key
// 将hash中指定域的值增加给定的数字

HINCRBY key field increment
// 将hash中指定域的值增加给定的浮点数

HINCRBYFLOAT key field increment
// 获取hash的所有字段

HKEYS key
// 获取hash里所有字段的数量

HLEN key
// 获取hash里面指定字段的值

HMGET key field [field ...]
// 设置hash字段值

HMSET key field value [field value ...]
// 设置hash里面一个字段的值

HSET key field value
// 设置hash的一个字段，只有当这个字段不存在时有效

HSETNX key field value
// 获取hash里面指定field的长度

HSTRLEN key field
// 获得hash的所有值

HVALS key
// 执行原子加1操作

INCR key
// 执行原子增加一个整数

INCRBY key increment
// 执行原子增加一个浮点数

INCRBYFLOAT key increment
// 获得服务器的详细信息

INFO [section]
// 查找所有匹配给定的模式的键

KEYS pattern
// 获得最后一次同步磁盘的时间

LASTSAVE
// 获取一个元素，通过其索引列表

LINDEX key index
// 在列表中的另一个元素之前或之后插入一个元素

LINSERT key BEFORE|AFTER pivot value
// 获得队列(List)的长度

LLEN key
// 从队列的左边出队一个元素

LPOP key
// 从队列的左边入队一个或多个元素

LPUSH key value [value ...]
// 当队列存在时，从队到左边入队一个元素

LPUSHX key value
// 从列表中获取指定返回的元素

LRANGE key start stop
// 从列表中删除元素

LREM key count value
// 设置队列里面一个元素的值

LSET key index value
// 修剪到指定范围内的清单

LTRIM key start stop
// Outputs memory problems report

MEMORY DOCTOR
// Show helpful text about the different subcommands

MEMORY HELP
// Show allocator internal stats

MEMORY MALLOC-STATS
// Ask the allocator to release memory

MEMORY PURGE
// Show memory usage details

MEMORY STATS
// Estimate the memory usage of a key

MEMORY USAGE key [SAMPLES count]
// 获得所有key的值

MGET key [key ...]
// 原子性的将key从redis的一个实例移到另一个实例

MIGRATE host port key destination-db timeout [COPY] [REPLACE]
// 实时监控服务器

MONITOR
// 移动一个key到另一个数据库

MOVE key db
// 设置多个key value

MSET key value [key value ...]
// 设置多个key value,仅当key存在时

MSETNX key value [key value ...]
// 标记一个事务块开始

MULTI
// 检查内部的再分配对象

OBJECT subcommand [arguments [arguments ...]]
// 移除key的过期时间

PERSIST key
// 设置key的有效时间以毫秒为单位

PEXPIRE key milliseconds
// 设置key的到期UNIX时间戳以毫秒为单位

PEXPIREAT key milliseconds-timestamp
// 将指定元素添加到HyperLogLog

PFADD key element [element ...]
// Return the approximated cardinality of the set(s) observed by the HyperLogLog at key(s).

PFCOUNT key [key ...]
// Merge N different HyperLogLogs into a single one.

PFMERGE destkey sourcekey [sourcekey ...]
// Ping 服务器

PING
// Set the value and expiration in milliseconds of a key

PSETEX key milliseconds value
// Listen for messages published to channels matching the given patterns

PSUBSCRIBE pattern [pattern ...]
// Inspect the state of the Pub/Sub subsystem

PUBSUB subcommand [argument [argument ...]]
// 获取key的有效毫秒数

PTTL key
// 发布一条消息到频道

PUBLISH channel message
// 停止发布到匹配给定模式的渠道的消息听

PUNSUBSCRIBE [pattern [pattern ...]]
// 关闭连接，退出

QUIT
// 返回一个随机的key

RANDOMKEY
// Enables read queries for a connection to a cluster slave node

READONLY
// Disables read queries for a connection to a cluster slave node

READWRITE
// 将一个key重命名

RENAME key newkey
// 重命名一个key,新的key必须是不存在的key

RENAMENX key newkey
// Make the server a replica of another instance, or promote it as master.

REPLICAOF host port
// Create a key using the provided serialized value, previously obtained using DUMP.

RESTORE key ttl serialized-value [REPLACE]
// Return the role of the instance in the context of replication

ROLE
// 从队列的右边出队一个元

RPOP key
// 删除列表中的最后一个元素，将其追加到另一个列表

RPOPLPUSH source destination
// 从队列的右边入队一个元素

RPUSH key value [value ...]
// 从队列的右边入队一个元素，仅队列存在时有效

RPUSHX key value
// 添加一个或者多个元素到集合(set)里

SADD key member [member ...]
// 同步数据到磁盘上

SAVE
// 获取集合里面的元素数量

SCARD key
// Set the debug mode for executed scripts.

SCRIPT DEBUG YES|SYNC|NO
// Check existence of scripts in the script cache.

SCRIPT EXISTS script [script ...]
// 删除服务器缓存中所有Lua脚本。

SCRIPT FLUSH
// 杀死当前正在运行的 Lua 脚本。

SCRIPT KILL
// 从服务器缓存中装载一个Lua脚本。

SCRIPT LOAD script
// 获得队列不存在的元素

SDIFF key [key ...]
// 获得队列不存在的元素，并存储在一个关键的结果集

SDIFFSTORE destination key [key ...]
// 选择新数据库

SELECT index
// 设置一个key的value值

SET key value [EX seconds] [PX milliseconds] [NX|XX]
// Sets or clears the bit at offset in the string value stored at key

SETBIT key offset value
// 设置key-value并设置过期时间（单位：秒）

SETEX key seconds value
// 设置的一个关键的价值，只有当该键不存在

SETNX key value
// Overwrite part of a string at key starting at the specified offset

SETRANGE key offset value
// 关闭服务

SHUTDOWN [NOSAVE] [SAVE]
// 获得两个集合的交集

SINTER key [key ...]
// 获得两个集合的交集，并存储在一个关键的结果集

SINTERSTORE destination key [key ...]
// 确定一个给定的值是一个集合的成员

SISMEMBER key member
// 指定当前服务器的主服务器

SLAVEOF host port
// 管理再分配的慢查询日志

SLOWLOG subcommand [argument]
// 获取集合里面的所有元素

SMEMBERS key
// 移动集合里面的一个元素到另一个集合

SMOVE source destination member
// 对队列、集合、有序集合排序

SORT key [BY pattern] [LIMIT offset count] [GET pattern] [ASC|DESC] [ALPHA] destination
// 删除并获取一个集合里面的元素

SPOP key [count]
// 从集合里面随机获取一个元素

SRANDMEMBER key [count]
// 从集合里删除一个或多个元素

SREM key member [member ...]
// 获取指定key值的长度

STRLEN key
// 监听频道发布的消息

SUBSCRIBE channel [channel ...]
// 添加多个set元素

SUNION key [key ...]
// 合并set元素，并将结果存入新的set里面

SUNIONSTORE destination key [key ...]
// Swaps two Redis databases

SWAPDB index index
// 用于复制的内部命令

SYNC
// 返回当前服务器时间

TIME
// Alters the last access time of a key(s). Returns the number of existing keys specified.

TOUCH key [key ...]
// 获取key的有效时间（单位：秒）

TTL key
// 获取key的存储类型

TYPE key
// Delete a key asynchronously in another thread. Otherwise it is just as DEL, but non blocking.

UNLINK key [key ...]
// 停止频道监听

UNSUBSCRIBE [channel [channel ...]]
// 取消事务命令

UNWATCH
// Wait for the synchronous replication of all the write commands sent in the context of the current connection

WAIT numslaves timeout
// 锁定key直到执行了 MULTI/EXEC 命令

WATCH key [key ...]
// Marks a pending message as correctly processed, effectively removing it from the pending entries list of the consumer group. Return value of the command is the number of messages successfully acknowledged, that is, the IDs we were actually able to resolve in the PEL.

XACK key group ID [ID ...]
// Appends a new entry to a stream

XADD key ID field string [field string ...]
// Changes (or acquires) ownership of a message in a consumer group, as if the message was delivered to the specified consumer.

XCLAIM key group consumer min-idle-time ID [ID ...] [IDLE ms] [TIME ms-unix-time] [RETRYCOUNT count] [FORCE] [JUSTID]
// Removes the specified entries from the stream. Returns the number of items actually deleted, that may be different from the number of IDs passed in case certain IDs do not exist.

XDEL key ID [ID ...]
// Create, destroy, and manage consumer groups.

XGROUP [CREATE key groupname id-or-$] [SETID key id-or-$] [DESTROY key groupname] [DELCONSUMER key groupname consumername]
// Get information on streams and consumer groups

XINFO [CONSUMERS key groupname] key key [HELP]
// Return the number of entires in a stream

XLEN key
// Return information and entries from a stream consumer group pending entries list, that are messages fetched but never acknowledged.

XPENDING key group [start end count] [consumer]
// Return a range of elements in a stream, with IDs matching the specified IDs interval

XRANGE key start end [COUNT count]
// Return never seen elements in multiple streams, with IDs greater than the ones reported by the caller for each stream. Can block.

XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] ID [ID ...]
// Return new entries from a stream using a consumer group, or access the history of the pending entries for a given consumer. Can block.

XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] ID [ID ...]
// Return a range of elements in a stream, with IDs matching the specified IDs interval, in reverse order (from greater to smaller IDs) compared to XRANGE

XREVRANGE key end start [COUNT count]
// Trims the stream to (approximately if '~' is passed) a certain size

XTRIM key MAXLEN [~] count
// 添加到有序set的一个或多个成员，或更新的分数，如果它已经存在

ZADD key [NX|XX] [CH] [INCR] score member [score member ...]
// 获取一个排序的集合中的成员数量

ZCARD key
// 返回分数范围内的成员数量

ZCOUNT key min max
// 增量的一名成员在排序设置的评分

ZINCRBY key increment member
// 相交多个排序集，导致排序的设置存储在一个新的关键

ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight] [SUM|MIN|MAX]
// 返回成员之间的成员数量

ZLEXCOUNT key min max
// Remove and return members with the highest scores in a sorted set

ZPOPMAX key [count]
// Remove and return members with the lowest scores in a sorted set

ZPOPMIN key [count]
// 根据指定的index返回，返回sorted set的成员列表

ZRANGE key start stop [WITHSCORES]
// 返回指定成员区间内的成员，按字典正序排列, 分数必须相同。

ZRANGEBYLEX key min max [LIMIT offset count]
// 返回指定成员区间内的成员，按字典倒序排列, 分数必须相同

ZREVRANGEBYLEX key max min [LIMIT offset count]
// 返回有序集合中指定分数区间内的成员，分数由低到高排序。

ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
// 确定在排序集合成员的索引

ZRANK key member
// 从排序的集合中删除一个或多个成员

ZREM key member [member ...]
// 删除名称按字典由低到高排序成员之间所有成员。

ZREMRANGEBYLEX key min max
// 在排序设置的所有成员在给定的索引中删除

ZREMRANGEBYRANK key start stop
// 删除一个排序的设置在给定的分数所有成员

ZREMRANGEBYSCORE key min max
// 在排序的设置返回的成员范围，通过索引，下令从分数高到低

ZREVRANGE key start stop [WITHSCORES]
// 返回有序集合中指定分数区间内的成员，分数由高到低排序。

ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]
// 确定指数在排序集的成员，下令从分数高到低

ZREVRANK key member
// 获取成员在排序设置相关的比分

ZSCORE key member
// 添加多个排序集和导致排序的设置存储在一个新的关键

ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [SUM|MIN|MAX]
// 增量迭代key

SCAN cursor [MATCH pattern] [COUNT count]
// 迭代set里面的元素

SSCAN key cursor [MATCH pattern] [COUNT count]
// 迭代hash里面的元素

HSCAN key cursor [MATCH pattern] [COUNT count]
// 迭代sorted sets里面的元素

ZSCAN key cursor [MATCH pattern] [COUNT count]
```





# #参考文献

[Link: 葡萄城|Redis是什么？](https://www.cnblogs.com/powertoolsteam/p/redis.html)