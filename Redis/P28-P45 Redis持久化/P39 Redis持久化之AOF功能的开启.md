### 开启AOF
appendonly yes

配置保存写回策略:appendfsync everysec
### 保存路径
6版本Redis的RDB文件和AOF文件保存路径一样

appenddirname "appendonlydir"
### 保存文件类型
6版本以前只保存一个AOF文件
7版本更新有
```sql
 - appendonly.aof.1.base.rdb as a base file.<--基本文件
 - appendonly.aof.1.incr.aof, appendonly.aof.2.incr.aof as incremental files.<--增量文件
 - appendonly.aof.manifest as a manifest file.<--清单文件
```