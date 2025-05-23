---
date: '2025-05-09T18:14:07+08:00'
draft: false
title: 'Redis'
seriesOpened: false #s是否开启系列
# series: [""] #属于的系列 
# series_order: 0  #系列编号
showSummary: true #摘要信息
tags: ["Redis"]
Categories: ["学习笔记"]
layoutBackgroundBlur: false #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---
## 字符串String

1. 设置

   ```SET key value```

2. 获取
   ```GET key```

3. 删除
   ```DEL key```

4. 判断是否存在
   ```EXISTS key```

5. 查看有哪些键
   ```KEYS [pattern模式匹配]```

6. 删除所有键
   ```FLUSHALL```

7. 查看过期时间
   ```TTL key```

8. 设置过期时间
   ```EXPIRE key time/秒```

9. 设置一个带有过期时间的键值对
   ```SETEX key time value```

10. 当键不存在时设置该键值对
    `SETNX key value`

## 列表List

存储或操作一组有顺序的数据

1. 尾部添加
   ```RPUSH```
2. 头部添加
   ```LPUSH```
3. 查看
   ```LRANGE key start stop```
4. 头删除
   ```RPOP key [删除元素个数]```
5. 尾删除
   ```LPOP key``` [删除元素个数]
6. 指定范围删除
   ```LTRIM key start stop```

## 集合Set

无序集合，不允许重复元素

1. 添加
   ```SADD key member```
2. 判断一个元素是否在集合中
   ```SISMEMBER key member```
3. 删除
   ```SREM key member```
4. 查看集合元素
   ```SMEMBERS key```

## 有序集合SortedSet

每个元素关联一个浮点类型的分数，按照分数对集合中的元素进行从小到大排序，成员是唯一的，分数是可以重复的

1. 添加
   ```ZADD key score1 value1 [score2 value2 ……]```

2. 查看

   ```c
   ZRANGE key start end [WITHSCORES]
   WITHSCORES：同时输出分数
   
   //查看分数
   ZSCORE key value
   
   //查看排名
   ZRANK key value
   ```


## 哈希Hash

   字符类型的字段和值的映射表，简单来说就是一个键值对的集合，特别适合存储对象

   1. 添加
      ```HEST key field1 value1 [fild2 value2 ……]```

   2. 获取
      ```HGET key field```

   3. 获取所有键值对
      ```HGETALL key```

   4. 删除键值对
      ```HEXISTS key field```

   5. 获取所有键
      ```HKEYS key```

   6. 获取键的数量

      ```HLEN key```

## 发布订阅模式

无法持久化，无法记录历史

发送：PUBLISH

接受：SUBSCRIBE 频道名称

## 消息队列Stream

轻量级消息队列，解决发布订阅功能的一些局限性，大部分命令用 X 开头

1. 添加消息
   ```XADD key * field value```

   \* 表示随机id

   id 格式：`整数-整数`，第一个整数表示时间戳，第二个整数表示一个序列号，在使用时，需要保证 id 是递增的

2. 消息数量
   ```XLEN key```

3. 消息详细内容
   ```XRANGE key start end```
   可以用 - + 表示显示所有内容

4. 删除
   ```XDEL key ID```

   也可以通过`XTRIM key MAXLEN `删除消息

5. 读取消息
   `XREAD [COUNT 消息数量] [BLOCK 阻塞时间] STREAMS key start`

   start 可以为$符，表示从执行时间开始阻塞，接受最新的消息

6. 创建消费者组

   `XGROUP CREATE 消息名称 组的名称 ID `

   通过 `XINFO GROUPS 消息名称`查看消费者组的信息

   添加消费者`XGROUP CREATECONSUMER 消息名字 组名字 消费者名字`

   读取消息：`XREADGROUP GROUP 组名称 消费者名称 COUNT 消息数量 BLOCK 阻塞的时间 STREAMS 消息名称`

## 地理空间Geospatial

存储地理位置信息的数据结构，支持对地理位置进行各种计算操作，命令以GEO开头

1. 添加

   `GEOADD 信息名字 经度 维度 城市名字 [经度 维度 城市名字 ……]`

2. 获取经纬度

   `GEOPOS 信息名字 城市名字`

3. 计算两个地理位置之间的距离

   `GEODIST 信息名字 城市名字1 城市名字2`

   默认单位是米，在后面加上km为千米

4. 搜索指定范围内的成员并返回

   `GEOSEARCH 信息名字 FROMMEMBER 城市名字1  `

## hyperloglog

做基数统计的算法，使用随机算法来计算，通过牺牲一定的精确度换取更小的内存消耗，优点占用内存小，缺点会有一定的误差，适合做一些对精确度要求不高，而数据量非常大的统计工作

命令以 PF 开头

1. 添加元素

   `PFADD 元素名 元素1 [元素2 …… 元素n]`

2. 查看基数

   `PFCOUNT 元素名`

3. 合并

   `PFMERGE new元素名 元素名1 元素名2`

## 位图Bitmap

由二进制位组成的数组，每个位只能是0或1，可以很方便的存储只有是否两个状态信息的数据

命令以bit开头

1. 创建

   `SETBIT key bit1 [bit2 …… bitn]`

2. 获取偏移量的值
   `GETBIT key 偏移量`

3. 批量设置

   `SET key "数值"`

## 位域Bitfield ？？？？？？？？？？？

将很多小的整数存储到一个较大的位图中

## 事务

不能保证所有命令执行成功，执行结果取决于事务中的命令

主要通过 MULTI 和 EXEC / DESCARD 实现

1. 在发送 EXEC 命令之前 ，所有命令都会放入一个队列中缓存起来不会立即执行
2. 在收到 EXEC 命令之后，事务开始执行，其中任何一个命令执行失败，其他命令依然会执行
3. 在事务执行过程中，其他客户端提交的命令请求不会被插入到事务的执行命令序列中

## 持久化

### RDB

在指定时间内，将内存中的数据快照写入磁盘，是某一个时间点上数据的完整副本，可以通过配置文件中的 save 参数来配置，如果宕机，在最后一次快照之后的数据全部丢失，所以这种方式只适合备份，当为redis开辟的空间比较大，那么写入磁盘的时间会很长，这期间redis处于阻塞状态，不能处理任何求情

为此，redis提供了一种 bgsave 命令，这个命令会创建一个单独的子进程负责将数据写入磁盘，但是这中间还会有性能损耗，因为fork一个子进程也需要时间，这期间redis也是不能响应请求

### AOF

在执行写命令的时候，不仅会写入数据，还会将命令写入到一个追加的文件中，这个文件就是AOF文件，已日志的形式记录每一个写操作，当redis重启之后，会通过执行 AOF 文件中的命令再内存中重建整个数据库的内容

### 开启方式

再配置文件中将 appendonly 这个参数的值改成 yes 就可以了

## 主从复制

将一台 redis 的数据复制到其他 redis 服务器，主节点和从节点为一对多关系，数据的复制为单向的，只能主到从，一般来说主负责写操作，从负责读操作

### 配置方式

默认为主节点，不需要配置

### 从节点配置

1. 把配置文件复制到根目录一份
2. 回到根目录，复制一个-6380的文件，作为从节点的配置文件，6380就是从节点的端口号
3. 将 port 改为6380，将pidfile、dbfilename 指向的文件后面添加-6380，pid是进程id，为了和主节点的pid文件区分开
4. 将replicaof 地址改为127.0.0.1，将端口号改为 6379 表示现在配置的这个节点是6379这个库的从节点

链接从节点 redis-cli -p 6380

链接成功后使用 info replication 查看链接信息

## 哨兵模式

自动故障转移，哨兵会以一个独立的进程运行再Redis集群中，用来监控Redis集群中的各个节点是否运行正常，主要功能有：

监控：通过不断地发送命令，检查Redis节点是否运行正常

通知：如果发现某个节点出现问题，那么哨兵就会通过发布订阅模式，通知其他节点

自动故障转移：当主节点不能正常工作的时候，哨兵会开始一个自动故障转移的操作，它会将一个从节点升级为新的主节点，然后再将其他从节点指向新的主节点

**配置步骤：**

~~~c
1、添加配置文件
vi sentinel.conf
	添加配置信息   1 表示需要一个哨兵节点同意
	sentinel monitor 主节点名称 127.0.0.1:6379 1
	
2、添加哨兵节点
redis-sentinel 配置文件名字

~~~

