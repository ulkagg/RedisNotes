![](image-20230807172357842.png)

+ AOF文件是只进行追加的日志文件
+ Redis在AOF文件体积变得过大时,自动对AOF文件在后台进行重写
+ AOF文件有序的保存了对数据库执行的所有写入操作,这些写入操作以Redis的协议格式保存,因此,AOF文件的内容非常容易被人读懂,分析轻松
+ 对于相同的数据集来说,AOF文件通常要大于RDB文件的体积
+ 根据所使用的的fsync策略,AOF的速度可能会慢于RDB