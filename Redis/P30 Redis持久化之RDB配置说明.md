### 自动触发
## Snapshotting(快照)
+ manually:手动
+ manually:手动触发
+ configure:配置

By default Redis saves snapshots of the dataset on disk, in a binary file called `dump.rdb`. You can ==configure== Redis to have it save the dataset every N seconds if there are at least M changes in the dataset, or you can ==manually call== the [`SAVE`](https://redis.io/commands/save) or [`BGSAVE`](https://redis.io/commands/bgsave) commands.
+ 修改保存频率
![400](image-20230804163815616.png)
+ 修改保存路径到/myredis/dumpfiles
![](image-20230804164157787.png)
+ 修改rdb文件名称
dbfilename dump6379.rdb
```sql
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) "926538"
127.0.0.1:6379> CONFIG get port
1) "port"
2) "6379"
127.0.0.1:6379> CONFIG get dir
1) "dir"
2) "/root"

```
+ 配置rdb文件夹
	配置redis7.conf,此配置文件不在`/opt/myredis/redis7.conf`,而在`/myredis/redis7.conf`
	找到redis7.conf,修改dir为`dir /myredis/dumpfiles`
	`ps -ef | grep redis`查看redis运行端口
	```sql
	[root@ulkagg dumpfiles]# ps -ef | grep redis
root       8818      1  0 12:01 ?        00:00:18 redis-server 127.0.0.1:6379
root      14241  13486  0 17:37 pts/1    00:00:00 grep --color=auto redis

	```
	`kill 8818`终止redis运行
	重启 `config get dir` 即可解决问题