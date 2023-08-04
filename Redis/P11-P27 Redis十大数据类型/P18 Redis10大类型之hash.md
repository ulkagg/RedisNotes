kv模式不变,但是v是一个键值对
###### hset/hget/hmset/hmget/hgetall/hdel
```sql
-->1.hset
127.0.0.1:6379> hset usr age 10 name lisi height 171
(integer) 3

-->2.hget
127.0.0.1:6379> hget usr name
"lisi"

-->3.hmset与hset一样

-->4.hmget
127.0.0.1:6379> hmget usr name height
1) "lisi"
2) "171"

-->5.hdel
127.0.0.1:6379> hdel usr age
(integer) 1
127.0.0.1:6379> hgetall usr
1) "name"
2) "lisi"
3) "height"
4) "171"
```
###### hlen 获取某个key内的全部数量
```sql
127.0.0.1:6379> hlen usr
(integer) 2
```
###### hexists 在key里面的是否存在某个值的key
```sql
127.0.0.1:6379> hexists usr age
(integer) 0
127.0.0.1:6379> hexists usr name
(integer) 1
```
###### hkeys/hvals 获取key中的v值中的对应键值/值
```sql
127.0.0.1:6379> hkeys usr
1) "name"
2) "height"
127.0.0.1:6379> hvals usr
1) "lisi"
2) "171"
```
###### hincrby/hincrbyfloat 自增数据整型/浮点型
```sql
-->1.自增hincrby数据整型
127.0.0.1:6379> hmget usr name height age rich
1) "lisi"
2) "171"
3) "18"
4) "2.56"
127.0.0.1:6379> HINCRBY usr age 1
(integer) 19
127.0.0.1:6379> HINCRBY usr age 1
(integer) 20
127.0.0.1:6379> HINCRBY usr age 1
(integer) 21
127.0.0.1:6379> hget usr age
"21"

-->2.hincrbyfloat自增浮点型
127.0.0.1:6379> HINCRBYFLOAT usr rich 0.01
"2.57"
127.0.0.1:6379> HINCRBYFLOAT usr rich 0.01
"2.58"
127.0.0.1:6379> HINCRBYFLOAT usr rich 0.01
"2.59"
127.0.0.1:6379> hget usr rich
"2.59"
```
###### hsetnx 若不存在则建立某个key的值中的键值对
```sql
127.0.0.1:6379> hsetnx usr bir 12.11
(integer) 1
127.0.0.1:6379> hsetnx usr bir 12.11
(integer) 0
127.0.0.1:6379> hget usr bir
"12.11"
127.0.0.1:6379> 
```