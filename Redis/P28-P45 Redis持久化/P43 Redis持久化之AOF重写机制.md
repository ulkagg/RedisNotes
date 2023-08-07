![](image-20230807172203705.png)


### 为什么要有AOF重写机制
AOF随着写操作越来越多,文件相对越来越大,占用服务器内存越大,以及AOF恢复要求时间越长
### 如何解决这种问题
- 为了解决这个问题,Redis新增了重新机制,当AOF文件的大小超过所设定的峰值时,Redis就会自动启动AOF文件的内容压缩,只保留可以恢复数据的最小指令集
- 或者
- 可以使用命令`bgrewriteaof`来更新
### 一句话总结为什么&怎么做
启动AOF文件内容压缩,只保留恢复数据的最小指令集

### 触发条件[#](https://www.cnblogs.com/StKali/p/13087736.html#%E8%A7%A6%E5%8F%91%E6%9D%A1%E4%BB%B6)
AOF文件触发条件可分为手动触发和自动触发：
#### 手动触发：客户端执行`bgrewriteaof`命令。
直接redis执行`bgrewriteaof`

#### 自动触发：自动触发通过以下两个配置协作生效：
`auto-aof-rewrite-min-size`
AOF文件最小重写大小，只有当AOF文件大小大于该值时候才可能重写,4.0默认配置64mb。
`auto-aof-rewrite-percentage`
当前AOF文件大小和最后一次重写后的大小之间的比率等于或者等于指定的增长百分比，如100代表当前AOF文件是上次重写的两倍时候才重写。　
##### 前置操作
设置AOF`auto-aof-rewrite-min-size`为1k
将RDB+AOF关闭,清除干扰项`aof-use-rdb-preamble no`
### 触发重写机制后
从1号增量文件变成2号增量文件
`append.aof.1.base.aof-->append.aof.2.base.aof`
`append.aof.1.incr.aof-->append.aof.2.incr.aof`
只保留最后一次操作
### 结论
AOF重写并不是对源文件重新整理,而是读取服务器现有的键值对,然后用一条命令去代替之前记录这个键值对的多条命令,生成一个新的文件替换原来的AOF文件
### 开启触发重写以后发生了什么
redis在AOF功能开启的情况下，会维持以下三个变量

`aof_current_size` ：记录当前AOF文件大小

`aof_rewrite_base_size`：记录最后一次AOF重写之后，AOF文件大小

`aof_rewrite_perc`：增长百分比变量

每次当serverCron（服务器周期性操作函数）函数执行时，它会检查以下条件是否全部满足，如果全部满足的话，就触发自动的AOF重写操作：

没有BGSAVE命令（RDB持久化）/AOF持久化在执行；

没有BGREWRITEAOF在进行；

当前AOF文件大小要大于server.aof_rewrite_min_size的值；

当前AOF文件大小和最后一次重写后的大小之间的比率等于或者大于指定的增长百分比（auto-aof-rewrite-percentage参数）