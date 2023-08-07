### 修复 redis-check-rdb
```sql
[root@ulkagg dumpfiles]# cd /usr/local/bin
[root@ulkagg bin]# ll
总用量 21552
-rwxr-xr-x. 1 root root  5205448 8月   4 11:51 redis-benchmark
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-aof -> redis-server
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-check-rdb -> redis-server
-rwxr-xr-x. 1 root root  5422864 8月   4 11:51 redis-cli
lrwxrwxrwx. 1 root root       12 8月   4 11:51 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 11436672 8月   4 11:51 redis-server

[offset 0] Checking RDB file /myredis/dumpfiles/dump6379.rdb
[offset 27] AUX FIELD redis-ver = '7.0.12'
[offset 41] AUX FIELD redis-bits = '64'
[offset 53] AUX FIELD ctime = '1691234570'
[offset 68] AUX FIELD used-mem = '1005192'
[offset 80] AUX FIELD aof-base = '0'
[offset 82] Selecting DB ID 0
[offset 122] Checksum OK
[offset 122] \o/ RDB looks OK! \o/
[info] 4 keys read
[info] 0 expires
[info] 0 already expired

```