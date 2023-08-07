### 1.人为 vim 破坏incr.aof文件
```sql

[root@ulkagg ~]# redis-server /myredis/redis7.conf
[root@ulkagg ~]# redis-cli -a 926538 -p 6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
Could not connect to Redis at 127.0.0.1:6379: Connection refused

```
### 2.cd /usr/local/bin查看命令
```sql
[root@ulkagg appendonlydir]# cd /usr/local/bin
[root@ulkagg bin]# ll
总用量 21552
-rwxr-xr-x. 1 root root  5205448 8月   4 11:51 redis-benchmark
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-aof -> redis-server
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-rdb -> redis-server
-rwxr-xr-x. 1 root root  5422864 8月   4 11:51 redis-cli
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 11436672 8月   4 11:51 redis-server

```
### 3.进行修复 redis-check-aof --fix(记得加--fix)
```sql
[root@ulkagg appendonlydir]# redis-check-aof --fix appendonly.aof.1.incr.aof
Start checking Old-Style AOF
AOF appendonly.aof.1.incr.aof format error
AOF analyzed: filename=appendonly.aof.1.incr.aof, size=400, ok_up_to=376, ok_up_to_line=89, diff=24
This will shrink the AOF appendonly.aof.1.incr.aof from 400 bytes, with 24 bytes, to 376 bytes
Continue? [y/N]: y
Successfully truncated AOF appendonly.aof.1.incr.aof

```
### 4.查看修复效果
```sql
[root@ulkagg ~]# redis-server /myredis/redis7.conf
[root@ulkagg ~]# redis-cli -a 926538 -p 6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> keys *
1) "k2"
2) "k7"
3) "k1"
4) "k3"
5) "k4"
6) "k6"
7) "k5"

```