- **AOF** (Append Only File): AOF persistence logs every write operation received by the server. These operations can then be replayed again at server startup, reconstructing the original dataset. Commands are logged using the same format as the Redis protocol itself.

日志形式的AOF，将命令追加到文件。AOF可以将Redis执行的每一条写命令追加到磁盘文件(`appendonly.aof`)中,在redis启动时候优先选择从AOF文件恢复数据。因为要频繁的将每一个操作记录到文件中，所以开启AOF持久化会对性能有一定的影响，但是大部分情况下这个影响是可以接受的。

与RDB持久化相比，AOF持久化数据丢失更少，其消耗内存更少(RDB方式执行bgsve会有内存拷贝)。