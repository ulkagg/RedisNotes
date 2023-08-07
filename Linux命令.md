### rm -f [filename]
删除文件命令
### rm -rf [filepathname]
删除文件夹命令
### cd /usr/local/bin ll 查看bin目录下的一些指令
```sql
[root@ulkagg appendonlydir]# cd /usr/local/bin
[root@ulkagg bin]# ll
总用量 21552
-rwxr-xr-x. 1 root root  5205448 8月   4 11:51 redis-benchmark
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-aof -> redis-server<--修复aof文件
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-rdb -> redis-server<--修复rdb文件
-rwxr-xr-x. 1 root root  5422864 8月   4 11:51 redis-cli
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 11436672 8月   4 11:51 redis-server

```