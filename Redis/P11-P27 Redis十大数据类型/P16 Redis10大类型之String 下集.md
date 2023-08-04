###### 同时设置/获取多个键值 
MSET key value&MGET key/msetnx
msetnx全部成功才能输出结果,指令1表示k1存在,创建不成功k1,k4
```sql
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> mget k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4
(integer) 0
127.0.0.1:6379> msetnx k4 v4 k5 v5
(integer) 1
127.0.0.1:6379> mget k5 k4
1) "v5"
2) "v4"
3)
```
###### 获取指定区间范围内的值
getrange/setrange
GETRANGE k1 0 -1
SETRANGE k1 1 xxyy 从第2位开始将后续内容依次替换
```sql
127.0.0.1:6379> set k1 abcd1234
OK
127.0.0.1:6379> GETRANGE k1 0 -1
"abcd1234"
127.0.0.1:6379> SETRANGE k1 1 xxyy
(integer) 8
127.0.0.1:6379> get k1
"axxyy234"
127.0.0.1:6379>  set k1 apple
OK
127.0.0.1:6379> SETRANGE k1 1 tihuan
(integer) 7
127.0.0.1:6379> get k1
"atihuan"
127.0.0.1:6379> 
```
###### 数值增减
只限定于键值对值是数字
`INCRE key`
`INCRBY key increment` :increment为自增数字
`DECR key`
`DECRBY key increment` :increment为自增数字
```sql
127.0.0.1:6379> set k1 100
OK

127.0.0.1:6379> incr k1
(integer) 106

127.0.0.1:6379> incrby k1 3
(integer) 109

127.0.0.1:6379> incrby k1 3
(integer) 115

```
###### 获取字符串长度和内容追加
`STRLEN key`
`APPEND key value`
###### 分布式锁
`setnx key value`
`setex(set with expire)键秒值/setnx(set if not exist)`
```sql
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> EXPIRE k1 60
(integer) 1
127.0.0.1:6379> ttl k1
(integer) 46

set with expire等效于-->setex

127.0.0.1:6379> SETEX k1 60 v11
OK
127.0.0.1:6379> ttl k1
(integer) 55


127.0.0.1:6379> setnx k1 v11
(integer) 0
127.0.0.1:6379> get k1
"v11"
127.0.0.1:6379> setnx k1 v11
(integer) 0
```
###### getset命令
先获取k1的值,再次设置
```sql
127.0.0.1:6379> getset k1 v11
"v1"
127.0.0.1:6379> get k1
"v11"
127.0.0.1:6379> 
```