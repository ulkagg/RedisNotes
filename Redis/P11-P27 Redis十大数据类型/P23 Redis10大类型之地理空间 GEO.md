`三维-->二维-->一维-->二进-->base32编码`
###### GEOADD
geoadd 用于存储指定的地理空间位置，可以将一个或多个经度(longitude)、纬度(latitude)、位置名称(member)添加到指定的 key 中。
geoadd 语法格式如下：
GEOADD key longitude latitude member [longitude latitude member ...]

天安门:116.403963,39.915119
人民英雄纪念碑:116.404165,39.910966
```sql
127.0.0.1:6379> geoadd geo1 116.403963 39.915119 tiananmen
(integer) 1
127.0.0.1:6379> type geo1
zset
[root@iZ0jljbcr2i0gw4bfux4ioZ ~]# redis-cli -a 926538 --raw
127.0.0.1:6379> zrange geo1 0 -1
tiananmen
天安门
```
###### geopos
geopos 用于从给定的 key 里返回所有指定名称(member)的位置（经度和纬度），不存在的返回 nil。
geopos 语法格式如下：
GEOPOS key member [member ...]
```sql
127.0.0.1:6379> geopos geo1 人民英雄纪念碑 天安门 tiananmen
116.40416711568832397
39.91096529540622129
116.40396326780319214
39.91511970338637383
116.40396326780319214
39.91511970338637383
```
###### geodist
geodist 用于返回两个给定位置之间的距离。
geodist 语法格式如下：
GEODIST key member1 member2 [m|km|ft|mi]

member1 member2 为两个地理位置。
最后一个距离单位参数说明：
- m ：米，默认单位。
- km ：千米。
- mi ：英里。
- ft ：英尺。
```sql
127.0.0.1:6379> geodist geo1 天安门 人民英雄纪念碑
462.4066
127.0.0.1:6379> geodist geo1 天安门 人民英雄纪念碑 km
0.4624
```
###### georadius、georadiusbymember

georadius 以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

georadiusbymember 和 GEORADIUS 命令一样， 都可以找出位于指定范围内的元素， 但是 georadiusbymember 的中心点是由给定的位置元素决定的， 而不是使用经度和纬度来决定中心点。

georadius 与 georadiusbymember 语法格式如下：
`GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]`
`GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]`
- WITHDIST: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。
- WITHCOORD: 将位置元素的经度和纬度也一并返回。
- WITHHASH: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。
- COUNT 限定返回的记录数。
	- `COUNT 10`返回10条记录
- ASC: 查找结果根据距离从近到远排序。
- DESC: 查找结果根据从远到近排序。
```sql
127.0.0.1:6379> georadius geo1 116.40416711568832397 39.91096529540622129 6 KM
人民英雄纪念碑
tiananmen
天安门
127.0.0.1:6379> georadiusbymember geo1 tiananmen  6 KM
人民英雄纪念碑
tiananmen
天安门127.0.0.1:6379> georadiusbymember geo1 tiananmen  6 KM withcoord withdist
人民英雄纪念碑
0.4624
116.40416711568832397
39.91096529540622129
tiananmen
0.0000
116.40396326780319214
39.91511970338637383
天安门
0.0000
116.40396326780319214
39.91511970338637383

```
### geohash 生成base32的编码值.三维变二维变一维

Redis GEO 使用 geohash 来保存地理位置的坐标。
geohash 用于获取一个或多个位置元素的 geohash 值。
geohash 语法格式如下：
GEOHASH key member [member ...]
```sql
127.0.0.1:6379> GEOHASH geo1 tiananmen 人民英雄纪念碑
wx4g0f6f2v0
wx4g0cdfjx0
```