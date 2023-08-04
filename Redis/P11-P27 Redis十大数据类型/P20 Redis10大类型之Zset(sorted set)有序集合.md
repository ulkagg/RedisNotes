###### 理论基础
在set基础上,每个val值钱加一个score分数值
之前set是k1 v1 v2 v3
现在zset是k1 score v1 score2 v2
###### ZADD key score member[score member ...]添加元素
```sql
127.0.0.1:6379> ZADD zset1 10 v1 20 v2 30 v3 40 v4 50 v5
(integer) 5
```
###### zrange key start stop [withscores] 按照元素从小到大的顺序 返回索引从start到stop之间的所有元素
```sql
127.0.0.1:6379> ZRANge zset1 0 -1 withscores
 1) "v1"
 2) "10"
 3) "v2"
 4) "20"
 5) "v3"
 6) "30"
 7) "v4"
 8) "40"
 9) "v5"
10) "50"
```
###### zrevrange 倒序
```sql
127.0.0.1:6379> ZREVRANGE zset1 0 -1 withscores
 1) "v5"
 2) "50"
 3) "v4"
 4) "40"
 5) "v3"
 6) "30"
 7) "v2"
 8) "20"
 9) "v1"
10) "10"
```
###### ZRANGEBYSCORE key min max [WITHSCORES]  [LIMIT offset count]
```sql
1.无withscores
127.0.0.1:6379> ZRANGEBYscore zset1 0 30
1) "v1"
2) "v2"
3) "v3"

2.有withscores
127.0.0.1:6379> ZRANGEBYscore zset1 0 30 withscores
1) "v1"
2) "10"
3) "v2"
4) "20"
5) "v3"
6) "30"

3."("不包含
127.0.0.1:6379> ZRANGEBYscore zset1 (10 30
1) "v2"
2) "v3"

4.limit限制下标
从0走1步
127.0.0.1:6379> zrangebyscore zset1 10 30 limit 0 1
1) "v1"
从0走2步
127.0.0.1:6379> zrangebyscore zset1 10 30 limit 0 2
1) "v1"
2) "v2"
```
###### ZSCORE key member 获取键元素对应排序值
```sql
127.0.0.1:6379> zscore zset1 v5
"50"
127.0.0.1:6379> zscore zset1 v4
"40"
```
###### ZCARD key 获取键值键数量
```sql
127.0.0.1:6379> zcard zset1
(integer) 5
```
###### ZREM key member[member...]某score下对应的value值,作用是删元素
```sql
1.删除v5对应的元素
127.0.0.1:6379> ZREM zset1 v5
(integer) 1

2.查看排序
127.0.0.1:6379> ZRANGE zset1 0 -1 withscores
1) "v1"
2) "10"
3) "v2"
4) "20"
5) "v3"
6) "30"
7) "v4"
8) "40"
```
###### ZINCRBY key increment member
```sql
1.元素处理
127.0.0.1:6379> ZINCRBY zset1 1 v1
"11"
127.0.0.1:6379> ZINCRBY zset1 1 v1
"12"
127.0.0.1:6379> ZINCRBY zset1 1 v1
"13"
127.0.0.1:6379> ZINCRBY zset1 1 v1
"14"

2.查询
127.0.0.1:6379> ZRANge zset1 0 -1 withscores
1) "v1"
2) "14"
3) "v2"
4) "20"
5) "v3"
6) "30"
7) "v4"
8) "40"
```
###### ZCOUNT key min max 获取指定分数范围的元素个数
```sql
127.0.0.1:6379> zcount zset1 15 30
(integer) 2
```
###### ZMPOP numkeys(弹出个数) key [key ...] MIN|MAX [COUNT count]
```sql
127.0.0.1:6379> zmpop 1 zset1 min count 1
1) "zset1"
2) 1) 1) "v1"
      2) "14"
127.0.0.1:6379> ZRANGE zset1 0 -1 withscores
1) "v2"
2) "20"
3) "v3"
4) "30"
5) "v4"
6) "40"
```
###### zrank key values
```sql
127.0.0.1:6379> zrank zset1 v5
(integer) 4
```
###### zrevrank key values
```sql
127.0.0.1:6379> zrevrank zset1 v5
(integer) 0
```