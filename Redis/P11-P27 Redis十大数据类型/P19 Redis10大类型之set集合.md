###### set集合与list区别
set集合是要无统统无,无序无重复
List是要有通通有,有序有重复[P17 Redis10大类型之List](P17%20Redis10大类型之List.md)
##### SADD key member[member...] 添加元素
```sql
127.0.0.1:6379> sadd set1 111 111 2 2 3 3 4 55 55
(integer) 5
```
###### SMEMBERS key 遍历集合中所有元素
```sql
127.0.0.1:6379> SMEMBERS set1
1) "2"
2) "3"
3) "4"
4) "55"
5) "111"
```
###### SISMEMBER key member 判断元素是否在集合中
```sql
127.0.0.1:6379> sismember set1 111
(integer) 1
```
###### SREM key member [member...] 删除元素
```sql
127.0.0.1:6379> srem set1 111 3 4 5
(integer) 3
127.0.0.1:6379> SMEMBERS set1
1) "2"
2) "55"
```
###### scard 获取集合里面的元素个数
```sql
127.0.0.1:6379> scard set1
(integer) 2
```
###### SRANDMEMBER key [数字] 从集合中随即展现设置数字个数元素,元素不删除
```sql
127.0.0.1:6379> SRANDMEMBER set1 2
1) "2"
2) "55"
127.0.0.1:6379> SRANDMEMBER set1 1
1) "2"
```
###### SPOP key [数字] 从集合中随机弹出一个元素,出一个删一个
```sql
127.0.0.1:6379> spop set1 1
1) "2"
127.0.0.1:6379> spop set1 1
1) "55"
127.0.0.1:6379> SMEMBERS set1
(empty array)
```
###### smove key1 key2 在key1里存在的某个值,将k1里面存在的某个值赋值key2
```sql
127.0.0.1:6379> sadd set1 1 2 3 4 5
(integer) 5
127.0.0.1:6379> sadd set2 a b c d e
(integer) 5
127.0.0.1:6379> smove set1 set2 1
(integer) 1
127.0.0.1:6379> SMEMBERS set1
1) "2"
2) "3"
3) "4"
4) "5"
127.0.0.1:6379> SMEMBERS set2
1) "c"
2) "b"
3) "a"
4) "d"
5) "e"
6) "1"
```
###### SDIFF key1 key2 差集运算 A-B 属于A但不属于B
```sql
127.0.0.1:6379> sadd set1 1 2 3 a b
(integer) 5
127.0.0.1:6379> sadd set2 1 2 a b x
(integer) 5
127.0.0.1:6379> SDIFF set1 set2
1) "3"
```
###### sinter key1 key2 交集 A∩B
```sql
127.0.0.1:6379> sinter set1 set2
1) "a"
2) "1"
3) "2"
4) "b"
```
###### sunion key1 key2 并集 A∪B
```sql
127.0.0.1:6379> sunion set1 set2
1) "a"
2) "3"
3) "1"
4) "b"
5) "2"
6) "x"
```
