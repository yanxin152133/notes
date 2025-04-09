# 1. redis 命令
## 1.1. 登录
### 1.1.1. 默认用户`default`
```bash
root@edd3df5dd400:/data# redis-cli -h localhost -p 6379 -a "PASSWORD"
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
localhost:6379> ping
PONG
```

### 1.1.2. 其他用户
```bash
root@edd3df5dd400:/data# redis-cli -h localhost -p 6379  
localhost:6379> auth "TestAccount" "TestAccountPassword"
OK
localhost:6379> ping
PONG
```

## 1.2. redis服务器的各种信息和统计数据
```bash
localhost:6379> info
# Server
redis_version:7.2.3              ## redis服务器版本
redis_git_sha1:00000000          ## redis SHA1
redis_git_dirty:0                ## redis dirty flag
redis_build_id:4c883fe1b9c8a6e
redis_mode:standalone
os:Linux 5.15.0-91-generic x86_64        ## redis 服务器的宿主操作系统
arch_bits:64                             ## 架构
monotonic_clock:POSIX clock_gettime
multiplexing_api:epoll                   ## redis 所使用的事件处理机制
atomicvar_api:c11-builtin
gcc_version:12.2.0                       ## 编译redis时所使用的gcc版本
process_id:1                             ## 服务器进程的PID
process_supervised:no
run_id:2ef9cf6cd752ae198eec4f7df2d5ebe3e156ca9f          ## 服务器的随机标识符（用于Sentinel和集群）
tcp_port:6379                            ## TCP/IP监听端口
server_time_usec:1702624114257503
uptime_in_seconds:18081                  ## 自redis 服务器启动以来经过的秒数
uptime_in_days:0                         ## 自redis 服务器启动以来经过的天数
hz:10
configured_hz:10
lru_clock:8125298                        ## 以分钟为单位进行自增的时钟，用于 LRU 管理
executable:/data/redis-server
config_file:/etc/redis/redis.conf
io_threads_active:0
listener0:name=tcp,bind=*,bind=-::*,port=6379

# Clients
connected_clients:1                      ## 已连接客户端的数量（不包括通过从属服务器连接的客户端）
cluster_connections:0
maxclients:10000
client_recent_max_input_buffer:8
client_recent_max_output_buffer:0
blocked_clients:0                        ## 正在等待阻塞命令（BLPOP、BRPOP、BRPOPLPUSH）的客户端的数量
tracking_clients:0
clients_in_timeout_table:0
total_blocking_keys:0
total_blocking_keys_on_nokey:0

# Memory
used_memory:1017600                      ## 由 Redis 分配器分配的内存总量，以字节（byte）为单位
used_memory_human:993.75K                ## 以人类可读的格式返回 Redis 分配的内存总量
used_memory_rss:5107712                  ## 从操作系统的角度，返回 Redis 已分配的内存总量（俗称常驻集大小）。这个值和 top 、 ps 等命令的输出一致。
used_memory_rss_human:4.87M
used_memory_peak:1236248                 ## Redis 的内存消耗峰值（以字节为单位）
used_memory_peak_human:1.18M             ## 以人类可读的格式返回 Redis 的内存消耗峰值
used_memory_peak_perc:82.31%
used_memory_overhead:868128
used_memory_startup:866008
used_memory_dataset:149472
used_memory_dataset_perc:98.60%
allocator_allocated:1599936
allocator_active:1921024
allocator_resident:4591616
total_system_memory:2059173888
total_system_memory_human:1.92G
used_memory_lua:31744                    ## Lua 引擎所使用的内存大小（以字节为单位）
used_memory_vm_eval:31744
used_memory_lua_human:31.00K
used_memory_scripts_eval:0
number_of_cached_scripts:0
number_of_functions:0
number_of_libraries:0
used_memory_vm_functions:32768
used_memory_vm_total:64512
used_memory_vm_total_human:63.00K
used_memory_functions:184
used_memory_scripts:184
used_memory_scripts_human:184B
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.20
allocator_frag_bytes:321088
allocator_rss_ratio:2.39
allocator_rss_bytes:2670592
rss_overhead_ratio:1.11
rss_overhead_bytes:516096
mem_fragmentation_ratio:5.13                   ## used_memory_rss 和 used_memory 之间的比率
mem_fragmentation_bytes:4112408
mem_not_counted_for_evict:8
mem_replication_backlog:0
mem_total_replication_buffers:0
mem_clients_slaves:0
mem_clients_normal:1928
mem_cluster_links:0
mem_aof_buffer:8
mem_allocator:jemalloc-5.3.0
active_defrag_running:0
lazyfree_pending_objects:0
lazyfreed_objects:0

# Persistence   RDB 和 AOF 的相关信息
loading:0
async_loading:0
current_cow_peak:0
current_cow_size:0
current_cow_size_age:0
current_fork_perc:0.00
current_save_keys_processed:0
current_save_keys_total:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1702606033
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:-1
rdb_current_bgsave_time_sec:-1
rdb_saves:0
rdb_last_cow_size:0
rdb_last_load_keys_expired:0
rdb_last_load_keys_loaded:0
aof_enabled:1
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_rewrites:0
aof_rewrites_consecutive_failures:0
aof_last_write_status:ok
aof_last_cow_size:0
module_fork_in_progress:0
module_fork_last_cow_size:0
aof_current_size:88
aof_base_size:88
aof_pending_rewrite:0
aof_buffer_length:0
aof_pending_bio_fsync:0
aof_delayed_fsync:0

# Stats  一般统计信息
total_connections_received:19
total_commands_processed:22
instantaneous_ops_per_sec:0
total_net_input_bytes:1324
total_net_output_bytes:426456
total_net_repl_input_bytes:0
total_net_repl_output_bytes:0
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
instantaneous_input_repl_kbps:0.00
instantaneous_output_repl_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
expire_cycle_cpu_milliseconds:214
evicted_keys:0
evicted_clients:0
total_eviction_exceeded_time:0
current_eviction_exceeded_time:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
pubsubshard_channels:0
latest_fork_usec:0
total_forks:0
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0
total_active_defrag_time:0
current_active_defrag_time:0
tracking_total_keys:0
tracking_total_items:0
tracking_total_prefixes:0
unexpected_error_replies:0
total_error_replies:41
dump_payload_sanitizations:0
total_reads_processed:78
total_writes_processed:65
io_threaded_reads_processed:0
io_threaded_writes_processed:0
reply_buffer_shrinks:11
reply_buffer_expands:2
eventloop_cycles:180525
eventloop_duration_sum:15509398
eventloop_duration_cmd_sum:23661
instantaneous_eventloop_cycles_per_sec:9
instantaneous_eventloop_duration_usec:84
acl_access_denied_auth:2
acl_access_denied_cmd:0
acl_access_denied_key:0
acl_access_denied_channel:0

# Replication   主/从复制信息
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:c5cd4a80010d9c4b9cf4622f819c4a519eb759f0
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU    CPU 计算量统计信息
used_cpu_sys:9.867102
used_cpu_user:8.545348
used_cpu_sys_children:0.001317
used_cpu_user_children:0.001643
used_cpu_sys_main_thread:9.864331
used_cpu_user_main_thread:8.545662

# Modules

# Errorstats
errorstat_ERR:count=8
errorstat_NOAUTH:count=31
errorstat_WRONGPASS:count=2

# Cluster    Redis 集群信息
cluster_enabled:0

# Keyspace   数据库相关的统计信息
```