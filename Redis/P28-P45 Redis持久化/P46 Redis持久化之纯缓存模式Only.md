减少服务器占用资源,专心内存服务器功能
### 禁用RDB+AOF
禁用RDB:`Save ""`
仍然可以使用命令save,bgsave生成rdb文件
禁用AOF:appendonly no
仍然可以使用bgwriteaof生成aof文件