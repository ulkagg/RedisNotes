1. 需求:统计网站UV
	1. 什么是UV:Unique Vistor 独立访客,一般理解为客户端IP
	2. **需要去重考虑**
2. 去重复统计的基数估计算法-就是HyperLogLog
3. 在Redis里面,每个HyperLogLog键只需要划分12KB内存,就可以计算接近2^64个不同元素的基数
4. 基数
	1. 一种数据集,去重复后的真实个数
5. 基数统计:用于统计一个集合中不重复的元素个数,就是对集合去重复后剩余元素的计算
6. 标准误差:0.81%
###### PFADD key element[element...]
```sql
127.0.0.1:6379> PFadd danlianzhi 5 2 1
(integer) 1
127.0.0.1:6379> PFadd danlianzhi 5 2 1 521 52 1
(integer) 1
127.0.0.1:6379> PFcount danlianzhi
(integer) 5
127.0.0.1:6379> PFadd ser2 1 5 5 3 6 8
(integer) 1
127.0.0.1:6379> PFMERGE destkey danlianzhi ser2
OK
127.0.0.1:6379> PFCOUNT destkey
(integer) 8
```
######  PFCOUNT key 计算键基数
```sql
127.0.0.1:6379> PFcount danlianzhi
(integer) 5
127.0.0.1:6379> PFadd ser2 1 5 5 3 6 8
(integer) 1
127.0.0.1:6379> PFMERGE destkey danlianzhi ser2
OK
127.0.0.1:6379> PFCOUNT destkey
(integer) 8
```
###### PFMERGE destkey sourcekey [sourcekey...]
```sql
127.0.0.1:6379> PFMERGE destkey danlianzhi ser2
OK
127.0.0.1:6379> PFCOUNT destkey
(integer) 8
```
+ 用途:IP合并
	+ 第一天的有效IP与第二天的有效IP合并起来
	+ 计算电商访问次数