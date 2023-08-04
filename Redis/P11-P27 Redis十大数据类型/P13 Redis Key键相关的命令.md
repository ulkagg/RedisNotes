###### `keys *` 查看当前库所有的key
###### `exists key` 判断某个key是否存在
(integer) 1 存在一个,底层c语言编写
```sql
127.0.0.1:6379> EXISTS k1 k2 k3 k4 k5
(integer) 3
```

###### `type key`查看某个key是什么类型
```sql
127.0.0.1:6379> type k1
string
127.0.0.1:6379> lpush list 123 456 789
(integer) 3
127.0.0.1:6379> type list
list
```

###### `del key`删除某个key
```sql
127.0.0.1:6379> del k4
(integer) 0
127.0.0.1:6379> del k3
(integer) 1
```
###### `unlink key`非阻塞删除,仅仅将keys从keyspace元数据中删除,真正的删除会在后续异步中操作
```sql
127.0.0.1:6379> UNLINK k2
(integer) 1
```
###### `ttl key` 查看还有多少秒过期,-1表示永不过期,-2表示已经过期
```sql
127.0.0.1:6379> ttl k1
(integer) -1
127.0.0.1:6379> EXPIRE k1 5
(integer) 1
127.0.0.1:6379> ttl k1
(integer) -2
```
###### `expire key` 秒钟 为给定的key设置过期时间
```sql
127.0.0.1:6379> ttl k1
(integer) -1
127.0.0.1:6379> EXPIRE k1 5
(integer) 1
127.0.0.1:6379> ttl k1
(integer) -2
```
###### `move key dbindex[0-15]` 将当前数据库的key移动到给定的数据库db当中
Redis默认有16个数据库,默认使用的是0号库
```sql
127.0.0.1:6379> keys *
1) "\xe5\x8e\x9f\xe7\xa5\x9e\xe5\x90\xaf\xe5\x8a\xa8!"
2) "list"
3) "aaa"
4) "k5"
5) "k2"
6) "k6"
7) "k1"
8) "\xac\xed\x00\x05t\x00\x05hello"
127.0.0.1:6379> move list 3
(integer) 1
127.0.0.1:6379> select 3
OK
127.0.0.1:6379[3]> keys *
1) "list"
```
###### `select dbindex` 切换数据库[0-15],默认为0
`select 5`
###### `dbsize` 查看当前数据库key的数量
```sql
127.0.0.1:6379> select 3
OK
127.0.0.1:6379[3]> dbsize
(integer) 1


127.0.0.1:6379[3]> select 0
OK
127.0.0.1:6379> dbsize
(integer) 8
```
###### `flushdb` 清空当前库
```sql
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> keys *
1) "\xe5\x8e\x9f\xe7\xa5\x9e\xe5\x90\xaf\xe5\x8a\xa8!"
2) "list"
3) "aaa"
4) "k5"
5) "k2"
6) "k6"
7) "k1"
8) "\xac\xed\x00\x05t\x00\x05hello"
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> 
```
###### `flushall` 通杀所有库
[0-15]号库都删除