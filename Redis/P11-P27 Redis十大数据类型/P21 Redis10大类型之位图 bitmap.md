bit arrays(or simply bitmaps,我们称之为位图)
用String类型作为底层数据结构实现的一种统计二值状态的数据类型
位图的本质是数组,类型是String
###### SETBIT key offset(偏移) value(值)
```sql
01000001
127.0.0.1:6379> setbit k1 1 1
(integer) 0
127.0.0.1:6379> setbit k1 7 1
(integer) 0
127.0.0.1:6379> get k1
"A"
```
###### getbit key offset(检查第几位是否是0/1)
设置成什么时候来的天数
```sql
127.0.0.1:6379> getbit k1 365
(integer) 0
```
证明第365天没来
###### STRLEN key 统计数据字节数
8位一组的扩容
```sql
127.0.0.1:6379> setbit k1 3 1
(integer) 0
127.0.0.1:6379> strlen k1
(integer) 1
127.0.0.1:6379> setbit k1 10 1
(integer) 0
127.0.0.1:6379> strlen k1
(integer) 2
```
###### bitcount key 统计全部键里有多少个1
```sql
127.0.0.1:6379> bitcount k1
(integer) 5
```
###### bitcount key start end
```sql
127.0.0.1:6379> setbit k4 0 1
(integer) 0
127.0.0.1:6379> setbit k4 1 1
(integer) 0
127.0.0.1:6379> setbit k4 2 1
(integer) 0
127.0.0.1:6379> setbit k4 3 1
(integer) 0
127.0.0.1:6379> setbit k4 30 1
(integer) 0
127.0.0.1:6379> bitcount setbit 0 30
(integer) 0
127.0.0.1:6379> bitcount k4 0 30
(integer) 5
```
###### bitop 对二进制存储数据进行位运算(AND OR NOT XOR)
```sql

1.2023.01.01 ID为 1 2 3 4 序号都来了
127.0.0.1:6379> setbit 20230101 1 1
(integer) 0
127.0.0.1:6379> setbit 20230101 2 1
(integer) 0
127.0.0.1:6379> getbit 20230101 1
(integer) 1
127.0.0.1:6379> getbit 20230101 2
(integer) 1
127.0.0.1:6379> setbit 20230101 3 1
(integer) 0
127.0.0.1:6379> setbit 20230101 4 1
(integer) 0

2.2023.01.02 ID为 1的来了2的没来
127.0.0.1:6379> setbit 20230102 1 1
(integer) 0
127.0.0.1:6379> setbit 20230102  2 0
(integer) 0
127.0.0.1:6379> bitcount 20230101
(integer) 4
127.0.0.1:6379> bitcount 20230102
(integer) 1

3.进行这四天与AND运算
127.0.0.1:6379> bitop and k3 20230102 20230102
(integer) 1
127.0.0.1:6379> bitcount k3
(integer) 1
```

