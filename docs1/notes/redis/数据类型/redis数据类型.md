# 1. redis 数据类型
![redis数据类型](https://pdai.tech/images/db/redis/db-redis-ds-1.jpeg)
    
redis支持五种数据类型：string（字符串）、hash（哈希）、list（列表）、set（集合）及zset（sorted set：有序集合）

| 结构类型                     | 结构存储的值                               | 结构的读写能力                                                                                                                           |
| ---------------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| string字符串                 | 可以是字符串、整数或浮点数                 | 对整个字符串或字符串的一部分进行操作；对整数或浮点数进行自增或自减操作                                                                   |
| list列表                     | 一个链表，链表上的每个节点都包含一个字符串 | 对链表的两段进行push和pop操作，读取单个或多个元素；根据值查找或删除元素                                                                  |
| set集合                      | 包含字符串的无序集合                       | 字符串的集合，包含基础的方法有看是否存在添加、获取、删除；还包含计算交集、并集、差集等                                                   |
| hash哈希                     | 包含键值对的无序散列表                     | 包含方法有添加、获取、删除单个元素                                                                                                       |
| zset（sorted set：有序集合） | 和散列一样，用于存储键值对                 | 字符串成员与浮点数分数之间的有序映射；元素的排列顺序由分数的大小决定；包含方法有添加、获取、删除单个元素以及根据分支范围或成员来获取元素 |

## 1.1. string 字符串
string类型 是redis最基本的类型，一个key对应一个value。一个键最大能存储512MB。
string类型是二进制安全的，可以包含任何数据，如数字、字符串、jpg图片或者序列化的对象。

![string字符串](https://pdai.tech/images/db/redis/db-redis-ds-3.png)

| 命令   | 简述                   | 使用              |
| ------ | ---------------------- | ----------------- |
| GET    | 获取存储在给定键中的值 | GET name          |
| SET    | 设置存储在给定键中的值 | SET name value    |
| DEL    | 删除存储在给定键中的值 | DEL name          |
| INCR   | 将键存储的值加1        | INCR key          |
| DECR   | 将键存储的值减1        | DECR key          |
| INCRBY | 将键存储的值加上整数   | INCRBY key amount |
| DECRBY | 将键存储的值减去整数   | DECRBY key amount |

## 1.2. list 列表
redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。
redis使用双端链表实现list。

![list列表](https://pdai.tech/images/db/redis/db-redis-ds-5.png)

| 命令   | 简述                                     | 使用             |
| ------ | ---------------------------------------- | ---------------- |
| RPUSH  | 将给定值推入到列表右端                   | RPUSH key value  |
| LPUSH  | 将给定值推入到列表左侧                   | LPUSH key value  |
| RPOP   | 从列表的右端弹出一个值，并返回被弹出的值 | RPOP key         |
| LPOP   | 从列表的左侧弹出一个值，并返回被弹出的值 | LPOP key         |
| LRANGE | 获取列表在给定范围上的所有值             | LRANGE key 0 -1  |
| LINDEX | 通过索引获取列表中的元素                 | LINDEX key index |

## 1.3. set 集合
set是string类型的无序集合。
Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

![set 集合](https://pdai.tech/images/db/redis/db-redis-ds-7.png)

| 命令      | 简述                              | 使用                 |
| --------- | --------------------------------- | -------------------- |
| SADD      | 向集合添加一个或多个成员          | SADD key value       |
| SCARD     | 获取集合的成员数                  | SCARD key            |
| SMEMBRES  | 返回集合中的所有成员              | SMEMBERS key member  |
| SISMEMBER | 判断member元素是否是集合key的成员 | SISMEMBER key member |

## 1.4. hash 哈希
hash是一个键值对集合。是一个string类型的field（字段）和value（值）的映射表，适合用于存储对象。

![hash](https://pdai.tech/images/db/redis/db-redis-ds-4.png)

| 命令    | 简述                                     | 使用                          |
| ------- | ---------------------------------------- | ----------------------------- |
| HSET    | 添加键值对                               | HSET hash-key sub-key1 value1 |
| HGET    | 获取指定散列键的值                       | HGET hash-key key1            |
| HGETALL | 获取散列中包含的所有键值对               | HGETALL hash-key              |
| HDEL    | 如果给定键存在于散列中，那么就移除这个键 | HDEL hash-key sub-key1        |

## 1.5. zset（sorted set：有序集合）
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

有序集合的成员是唯一的, 但分数(score)却可以重复。有序集合是通过两种数据结构实现：
1. 压缩列表（ziplist）: ziplist是为了提高存储效率而设计的一种特殊编码的双向链表。它可以存储字符串或者整数，存储整数时是采用整数的二进制而不是字符串形式存储。它能在O(1)的时间复杂度下完成list两端的push和pop操作。但是因为每次操作都需要重新分配ziplist的内存，所以实际复杂度和ziplist的内存使用量相关
2. 跳跃表（zSkiplist）: 跳跃表的性能可以保证在查找，删除，添加等操作的时候在对数期望时间内完成，这个性能是可以和平衡树来相比较的，而且在实现方面比平衡树要优雅，这是采用跳跃表的主要原因。跳跃表的复杂度是O(log(n))。

![zset](https://pdai.tech/images/db/redis/db-redis-ds-8.png)

| 命令   | 简述                                                     | 使用                           |
| ------ | -------------------------------------------------------- | ------------------------------ |
| ZADD   | 将一个带有给定分值的成员添加到有序集合里面               | ZADD zset-key 178 member1      |
| ZRANGE | 根据元素在有序集合中所处的位置，从有序集合中获取多个元素 | ZRANGE zset-key 0-1 withccores |
| ZREM   | 如果给定元素成员存在于有序集合中，那么就移除这个元素     | ZREM zset-key member1          |
