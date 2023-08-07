### 1.删除rdb文件和文件夹,来保证是AOF可以从文件中恢复备份数据
```sql
[root@ulkagg ~]# cd /myredis
[root@ulkagg myredis]# rm -rf dumpfiles
[root@ulkagg myredis]# rm -f dump6379.rdb
[root@ulkagg myredis]# ll
总用量 108
drwxr-xr-x. 2 root root    103 8月   7 14:54 appendonlydir
-rw-r--r--. 1 root root 106587 8月   7 14:54 redis7.conf
```
### 2.复制appendonlydir
```sql
[root@ulkagg myredis]# cp -r appendonlydir/ appendonlydir.bak
[root@ulkagg myredis]# ll
总用量 108
drwxr-xr-x. 2 root root    103 8月   7 14:54 appendonlydir
drwxr-xr-x. 2 root root    103 8月   7 15:04 appendonlydir.bak
-rw-r--r--. 1 root root 106587 8月   7 14:54 redis7.conf


```
### 3.清除数据库
```sql
127.0.0.1:6379> keys *
1) "k4"
2) "k3"
3) "k2"
4) "k1"
127.0.0.1:6379> flushdb
OK

```
### 4.解压appendonlydir.bak
```sql
[root@ulkagg myredis]# rm -rf appendonlydir
[root@ulkagg myredis]# ll
总用量 112
drwxr-xr-x. 3 root root    128 8月   7 15:13 appendonlydir.bak
-rw-r--r--. 1 root root     89 8月   7 15:17 dump6379.rdb
-rw-r--r--. 1 root root 106587 8月   7 14:54 redis7.conf
[root@ulkagg myredis]# mv appendonlydir.bak/ appendonlydir
[root@ulkagg myredis]# ll
总用量 112
drwxr-xr-x. 3 root root    128 8月   7 15:13 appendonlydir
-rw-r--r--. 1 root root     89 8月   7 15:17 dump6379.rdb
-rw-r--r--. 1 root root 106587 8月   7 14:54 redis7.conf

```
### 5.查看恢复数据
```sql
[root@ulkagg ~]# redis-server /myredis/redis7.conf
[root@ulkagg ~]# redis-cli -a 926538 -p 6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> keys *
1) "k4"
2) "k2"
3) "k1"
4) "k3"

```
### 6.遇到的问题
1.未删除appendonlydir
```sql
mv：是否覆盖"appendonlydir/appendonlydir.bak"？ y
mv: 无法将"appendonlydir.bak/" 移动至"appendonlydir/appendonlydir.bak": 文件已存在
```
### 7.appendonlydir哪份数据会变大 appendonly.aof.1.incr.aof
```sql
127.0.0.1:6379> set k5 v5
OK
127.0.0.1:6379> set k6 v6
OK

[root@ulkagg appendonlydir]# ll
总用量 12
-rw-r--r--. 1 root root  89 8月   7 15:13 appendonly.aof.1.base.rdb
-rw-r--r--. 1 root root 318 8月   7 15:22 appendonly.aof.1.incr.aof
-rw-r--r--. 1 root root  88 8月   7 15:13 appendonly.aof.manifest
[root@ulkagg appendonlydir]# ll
总用量 12
-rw-r--r--. 1 root root  89 8月   7 15:13 appendonly.aof.1.base.rdb
-rw-r--r--. 1 root root 347 8月   7 15:23 appendonly.aof.1.incr.aof
-rw-r--r--. 1 root root  88 8月   7 15:13 appendonly.aof.manifest

```
### 8.总结
1.记录增量文件`incr.aof`中,基本文件和清单文件大小不变
2.只记录写操作命令,对于读操作命令,增量文件`incr.aof`大小不变
