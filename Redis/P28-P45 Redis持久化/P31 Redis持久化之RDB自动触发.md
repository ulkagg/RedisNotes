### RDB自动触发
![](image-20230804175322953.png)

+ 5秒钟自动保存,间隔超过5s则不会保留

### 如何恢复
将备份文件(dump.rdb)移动到redis安装目录并启动服务即可
备份成功后故意用flushdb清空redis,看看是否可以恢复数据
物理恢复,一定要服务和备份分机隔离
```sql
[root@ulkagg dumpfiles]# ll
总用量 4
-rw-r--r--. 1 root root 108 8月   4 17:51 dump6379.rdb
[root@ulkagg dumpfiles]# mv dump6379.rdb dump6379.rdb.bak

```
### rm -f [filename]
删除命令
### 结论
清空执行`flushdb/flushall`时会自动保存一次dump.rdb文件
shutdown&&quit退出redis时会自动保存一次dump.rdb文件
当dump.rdb.bak恢复dump.rdb(执行修改名称/复制)重新访问Redis数据库数据恢复
```sql
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> shutdown
not connected> quit
[root@ulkagg dumpfiles]# redis-server /myredis/redis7.conf

[root@ulkagg dumpfiles]# rm -f dump6379.rdb
[root@ulkagg dumpfiles]# ll
总用量 8
-rw-r--r--. 1 root root  89 8月   4 18:50 dump6379.rdb
-rw-r--r--. 1 root root 108 8月   4 17:51 dump6379.rdb.bak
[root@ulkagg dumpfiles]# rm -f dump6379.rdb
[root@ulkagg dumpfiles]# ll
总用量 4
-rw-r--r--. 1 root root 108 8月   4 17:51 dump6379.rdb.bak
[root@ulkagg dumpfiles]# mv dump6379.rdb.bak dump6379.rdb
[root@ulkagg dumpfiles]# redis-cli -a 926538
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
127.0.0.1:6379> 
```
### 注意
不可以把备份文件dump.rdb和生产redis服务器放在同一台机器,必须分开各自储存
以防生产及物理损坏备份文件也挂了

