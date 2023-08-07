# SAVE[](http://redisdoc.com/persistence/save.html#save "Permalink to this headline")

> 可用版本： >= 1.0.0
> 
> 时间复杂度： O(N)， `N` 为要保存到数据库中的 key 的数量。

[SAVE](http://redisdoc.com/persistence/save.html#save) 命令执行一个同步保存操作，将当前 Redis 实例的所有数据快照(snapshot)以 RDB 文件的形式保存到硬盘。
一般来说，在生产环境==很少执行== [SAVE](http://redisdoc.com/persistence/save.html#save) 操作，因为==它会阻塞所有客户端==，保存数据库的任务通常由 [BGSAVE](http://redisdoc.com/persistence/bgsave.html#bgsave) 命令异步地执行。然而，如果负责保存数据的后台子进程不幸出现问题时， [SAVE](http://redisdoc.com/persistence/save.html#save) 可以作为保存数据的最后手段来使用。
请参考文档： [Redis 的持久化运作方式(英文)](http://redis.io/topics/persistence) 以获取更多消息。
## 返回值[](http://redisdoc.com/persistence/save.html#id2 "Permalink to this headline")

保存成功时返回 `OK` 。

## 代码示例[](http://redisdoc.com/persistence/save.html#id3 "Permalink to this headline")

redis> SAVE
OK
```sql
127.0.0.1:6379> set k3 v3
OK

[root@ulkagg dumpfiles]# rm dump6379.rdb
rm：是否删除普通文件 "dump6379.rdb"？y
127.0.0.1:6379> save
OK

[root@ulkagg dumpfiles]# ll
总用量 4
-rw-r--r--. 1 root root 115 8月   4 19:24 dump6379.rdb
```
## BGSAVE
1. Redis会在后台异步进行快照操作,不阻塞快照同时还可以响应客户端请求,该触发方式还会fork一个子进程由子进程复制持久化的过程
	1. `bgsave`“后台保存”。与save 最大的差异为它并不是由 `worker thread` 执行的，而是由redis `fork` 出子进程，交由子进程来完成持久化操作。
	2. `redis`在`fork`子进程这个时间段内 redis是`阻塞`的（此段时间不会响应客户端请求），当子进程创建完成以后redis才会响应客户端请求。
	3. 并且redis不会在控制台显示完成信息，只会写入日志。

<font color="blue">fork是什么?</font>
在Linux程序中,fork()会产生一个和父进程完全相同的子进程,但子进程在此后多会exec(执行)系统调用,出于效率考虑,尽量避免膨胀
![](image-20230805192211858.png)

```sql
127.0.0.1:6379> SEt k4 v4
OK
[root@ulkagg dumpfiles]# ll
总用量 4
-rw-r--r--. 1 root root 115 8月   4 19:24 dump6379.rdb

127.0.0.1:6379> bgsave
Background saving started

[root@ulkagg dumpfiles]# ll
总用量 4
-rw-r--r--. 1 root root 122 8月   5 19:22 dump6379.rdb

```
### LASTSAVE
可以通过lastsave命令获取最后一次成功执行快照的时间
```sql
127.0.0.1:6379> LASTSAVE
(integer) 1691234570
[root@ulkagg dumpfiles]# date -d @1691234570
2023年 08月 05日 星期六 19:22:50 CST

```
### date -d @时间戳
通过时间戳获取时间
```sql
[root@ulkagg dumpfiles]# date -d @1691234570
2023年 08月 05日 星期六 19:22:50 CST
```