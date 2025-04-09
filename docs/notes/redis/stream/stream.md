# 1. redis Stream
## 1.1. 结构
![redis stream结构](https://pdai.tech/images/db/redis/db-redis-stream-2.png)

`Consumer Group`:消费组，使用 XGROUP CREATE 命令创建，一个消费组有多个消费者(Consumer), 这些消费者之间是竞争关系。
`last_delivered_id`：游标，每个消费组会有个游标 last_delivered_id，任意一个消费者读取了消息都会使游标 `last_delivered_id`往前移动。
`pending_ids`：消费者(Consumer)的状态变量，作用是维护消费者的未确认的 id。 pending_ids 记录了当前已经被客户端读取的消息，但是还没有 ack (Acknowledge character：确认字符）。如果客户端没有ack，这个变量里面的消息ID会越来越多，一旦某个消息被ack，它就开始减少。这个pending_ids变量在Redis官方被称之为PEL，也就是Pending Entries List，这是一个很核心的数据结构，它用来确保客户端至少消费了消息一次，而不会在网络传输的中途丢失了没处理。
`消息ID`: 消息ID的形式是timestampInMillis-sequence，例如1527846880572-5，它表示当前的消息在毫米时间戳1527846880572时产生，并且是该毫秒内产生的第5条消息。消息ID可以由服务器自动生成，也可以由客户端自己指定，但是形式必须是整数-整数，而且必须是后面加入的消息的ID要大于前面的消息ID。
`消息内容`: 消息内容就是键值对，形如hash结构的键值对，这没什么特别之处。#

## 1.2. stream出现的原因
redis stream主要用于消息队列（MQ，Message Queue），redis本身是有一个redis发布订阅（pub/sub）来实现消息队列的功能，但是其有个缺点就是消息无法持久化，如果出现网络断开，redis宕机等，消息就会被丢弃。

redis stream提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。

## 1.3. 消息队列相关命令
| 命令      | 作用                                          |
| --------- | --------------------------------------------- |
| XADD      | 添加消息到队列末尾                            |
| XTRIM     | 限制stream的长度，如果已经超长会进行截取      |
| XDEL      | 删除消息                                      |
| XLEN      | 获取stream中的消息长度                        |
| XRANGE    | 获取消息列表（可以指定范围），忽略删除的消息  |
| XREVRANGE | 和XRANGE相比区别在于反向获取，ID从大到小      |
| XREAD     | 获取消息（阻塞/非阻塞），返回大于指定ID的消息 |

## 1.4. 消费者相关命令
| 命令               | 作用                             |
| ------------------ | -------------------------------- |
| XGROUP CREATE      | 创建消费者                       |
| XREADGROUP GROUP   | 读取消费者组中的消息             |
| XACK               | 将消息标记为“已处理”             |
| XGROUP SETID       | 为消费者组设置新的最后递送消息ID |
| XGROUP DELCONSUMER | 删除消费者                       |
| XGROUP DESTROY     | 删除消费组                       |
| XPENDING           | 显示待处理消息的相关信息         |
| XCLAIM             | 转移消息的归属权                 |
| XINFO              | 查看流和消费者组的相关信息       |
| XINFO GROUPS       | 打印消费者组的信息               |
| XINFO STREAM       | 打印流信息                       |