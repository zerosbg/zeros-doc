# redis总结

>redis是一个开源的,内存中的数据结构存储系统,它可以用作数据库,缓存和消息中间件。
特点：单进程,单线程,模型的KV数据库。支持多种类型的数据结构,如 字符串(strings),散列(hashes),集合(sets),
有序集合(sorted sets)与范围查询,bitmaps,hyperloglogs和地理空间(geospatial)索引半径查询。Redis内置了复制(replication),LUA脚本(Lua scripting)
LRU驱动时间(LRU eviction),事务(transactions)和不同级别的磁盘持久化(persistence),并通过Redis哨兵(Sentinel)和自动分区(cluster)提供高可用性(high availability)


问题：Redis为什么使用单进程单线程方式也这么快
- ***完全基于内存***
- ***数据结构简单,对数据操作也简单***
- [使用多路I/O复用模型](#多路I/O复用)

### 多路I/O复用









###注释：
内容来源于[redis中文网](http://www.redis.cn/)