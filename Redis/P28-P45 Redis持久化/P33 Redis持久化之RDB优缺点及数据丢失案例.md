### 官方文档
#### 优点
- RDB is a very compact single-file point-in-time representation of your Redis data. RDB files are perfect for backups. ==For instance you may want to archive your RDB files every hour for the latest 24 hours==, and to save an RDB snapshot every day for 30 days. This allows you to easily restore different versions of the data set in case of disasters.
- ==RDB is very good for disaster recovery==, being a single compact file that can be transferred to far data centers, or onto Amazon S3 (possibly encrypted).
- RDB maximizes Redis performances since the only work the Redis parent process needs to do in order to persist is forking a child that will do all the rest. The parent process will never perform disk I/O or alike.
- RDB allows faster restarts with big datasets compared to AOF.
- On replicas, RDB supports [partial resynchronizations after restarts and failovers](https://redis.io/topics/replication#partial-resynchronizations-after-restarts-and-failovers).
- ![](image-20230805202038366.png)

##### 总结
![](image-20230805202454003.png)


#### 缺点
- RDB is a very compact single-file point-in-time representation of your Redis data. RDB files are perfect for backups. For instance you may want to archive your RDB files every hour for the latest 24 hours, and to save an RDB snapshot every day for 30 days. This allows you to easily restore different versions of the data set in case of disasters.
- RDB is very good for disaster recovery, being a single compact file that can be transferred to far data centers, or onto Amazon S3 (possibly encrypted).
- RDB maximizes Redis performances since the only work the Redis parent process needs to do in order to persist is forking a child that will do all the rest. The parent process will never perform disk I/O or alike.
- RDB allows faster restarts with big datasets compared to AOF.
- On replicas, RDB supports [partial resynchronizations after restarts and failovers](https://redis.io/topics/replication#partial-resynchronizations-after-restarts-and-failovers).
![](image-20230805202729985.png)

##### 总结
![](image-20230805202834962.png)

