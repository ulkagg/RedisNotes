## 官方文档
### AOF advantages

- Using AOF Redis is much more durable: you can have different fsync policies: no fsync at all, fsync every second, fsync at every query. With the default policy of fsync every second, write performance is still great. fsync is performed using a background thread and the main thread will try hard to perform writes when no fsync is in progress, so you can only lose one second worth of writes.
- The AOF log is an append-only log, so there are no seeks, nor corruption problems if there is a power outage. Even if the log ends with a half-written command for some reason (disk full or other reasons) the redis-check-aof tool is able to fix it easily.
- Redis is able to automatically rewrite the AOF in background when it gets too big. The rewrite is completely safe as while Redis continues appending to the old file, a completely new one is produced with the minimal set of operations needed to create the current data set, and once this second file is ready Redis switches the two and starts appending to the new one.
- AOF contains a log of all the operations one after the other in an easy to understand and parse format. You can even easily export an AOF file. For instance even if you've accidentally flushed everything using the [`FLUSHALL`](https://redis.io/commands/flushall) command, as long as no rewrite of the log was performed in the meantime, you can still save your data set just by stopping the server, removing the latest command, and restarting Redis again.

### AOF disadvantages

- AOF files are usually bigger than the equivalent RDB files for the same dataset.
- AOF can be slower than RDB depending on the exact fsync policy. In general with fsync set to _every second_ performance is still very high, and with fsync disabled it should be exactly as fast as RDB even under high load. Still RDB is able to provide more guarantees about the maximum latency even in the case of a huge write load.

**Redis < 7.0**

- AOF can use a lot of memory if there are writes to the database during a rewrite (these are buffered in memory and written to the new AOF at the end).
- All write commands that arrive during rewrite are written to disk twice.
- Redis could freeze writing and fsyncing these write commands to the new AOF file at the end of the rewrite.
### 演示优点

![](image-20230807155734535.png)

 删除即可
 ![](image-20230807155920153.png)

无事发生
## 翻译
### 优点
![](image-20230807160425739.png)


### 缺点
![](image-20230807160354489.png)


## 总结
相同数据集而言aof文件占用内存远大于rdb,恢复速度慢于rdb
aof运行效率要慢于rdb,每秒同步策略效率较好,不同步效率与rdb相同