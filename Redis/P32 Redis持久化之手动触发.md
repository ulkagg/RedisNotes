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