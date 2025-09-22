# 1. 配置
Redis 的配置文件位于 Redis 安装目录下，文件名为 redis.conf。

## 1.1. 参数说明
- 使用yes启动守护进程：`daemonize yes`
- 指定监听端口：`port 6379`
- 绑定的主机地址：`bind 127.0.0.1`
- 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭关闭该功能：`timeout 300`
- 指定日志记录级别：`loglevel verbose` Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
- 指定在多长时间内，有多少次更新操作：`save <seconds> <changes>`
- 指定存储至本地数据库时是否压缩数据：`rdbcompression yes`
- 指定本地数据库存放目录：`dir ./`
- 设置redis连接密码：`requirepass foobared`
- 设置同一时间最大客户端连接数，默认为无限制：`maxclients 128`
- 指定redis最大内存限制：`maxmemory <bytes>`
- 指定是否在每次更新操作后进行日志记录：`appendonly no`
- 指定更新日志文件名：`appendfilename appendonly.aof`
- 指定更新日志条件，`no`表示等操作系统进行数据缓存同步到磁盘（快），`always`表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全），`everysec`表示每秒同步一次（折衷，默认值）：`appendfsync everysec`
- 指定是否启用虚拟内存机制，默认值为no：`vm-enabled no`
- 虚拟内存文件路径：`vm-swap-file /tmp/redis.swap`
- 将所有大于vm-max-memory 的数据存入虚拟内存：`vm-max-memory 0`
- 设置page的大小：`vm-page-size 32`
- 设置swap文件中的page数量：`vm-pages 134217728`
- 设置访问swap文件的线程数：`vm-max-threads 4`
- 设置在向客户端应答时，是否把较小的包合并为一个包发送：`glueoutputbuf yes`
- 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法：`hash-max-zipmap-entries 64` `hash-max-zipmap-value 512`
- 指定是否激活重置哈希：`activerehashing yes`
- 指定包含其它的配置文件，可以在同一主机上多个redis实例之间使用同一配置文件，而同时各个实例又拥有自己特定的配置文件：`include /path/to/local.conf`