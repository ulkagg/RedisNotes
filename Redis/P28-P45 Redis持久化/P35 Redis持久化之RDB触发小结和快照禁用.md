## RDB触发小结
### 配置文件中默认的快照配置
redis7.conf
save 5 2(5秒钟2次保存文件)
### 手动save/bgsave命令
### 执行flushall/flushdb命令也会产生dump.rdb文件,但里面是空白的
### 执行shutdown且没有设置开启AOF持久化
### 主从复制时,主节点主动触发

## RDB快照禁用
`redis-cli config set save ""`
+ 2.打开此命令
![](image-20230805204858359.png)

