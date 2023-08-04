##### set命令/中括号中内容可以省略
`SET key value [NX | XX] [GET] [EX seconds | PX milliseconds |`
`EXAT unix-time-seconds | PXAT unix-time-milliseconds | KEEPTTL]`
###### [NX|XX]
NX:如果不存在某键则创建
XX:如果存在某键则创建
```sql
127.0.0.1:6379> set k1 v1 xx
(nil) #此时不存在键 k1,xx命令无法创建
127.0.0.1:6379> set k1 v1 nx
OK #此时不存在键 k1,nx命令可以创建
127.0.0.1:6379> get k1
"v1"
127.0.0.1:6379> set k1 v1 nx
(nil) #此时已存在键k1,无法用nx再次创建

127.0.0.1:6379> set k1 v1xx xx
OK
127.0.0.1:6379> get k1
"v1xx" #此时已存在键k1,可以用xx再次创建
```
###### [GET]
在设置之前,先返回当前k1值,然后再用新的值覆盖,弹出老的值,修改为新的
```sql
127.0.0.1:6379> set k1 v1 get
"v1xx"
127.0.0.1:6379> get k1
"v1"
```
###### [EX seconds | PX milliseconds | EXAT unix-time-seconds | PXAT unix-time-milliseconds | KEEPTTL]`
```sql
1.EX seconds
127.0.0.1:6379> set k1 v1 ex 10
OK
127.0.0.1:6379> ttl k1
(integer) 5
127.0.0.1:6379> ttl k1
(integer) 1
127.0.0.1:6379> ttl k1
(integer) -2
---
2.PX milliseconds 
127.0.0.1:6379> set k1 v1 px 10000
OK
127.0.0.1:6379> ttl k1
(integer) 5
127.0.0.1:6379> ttl k1
(integer) 1
```
3.EXAT unix-time-seconds
```JAVA
//获取unix-time-seconds
public class Main {  
    public static void main(String[] args) {  
        System.out.println(Long.toString(System.currentTimeMillis() / 1000L));  
    }  
}
```
```sql
127.0.0.1:6379> set k1 v1 exat 1690273956
OK
127.0.0.1:6379> ttl k1
(integer) 74
127.0.0.1:6379> 
```
4.PXAT unix-time-milliseconds
上列/1000
5.KEEPTTL,续接过期时间
```sql
127.0.0.1:6379> set k1 v1 ex 60
OK
127.0.0.1:6379> ttl k1
(integer) 55
127.0.0.1:6379> set k1 v1keepttl keepttl  
OK
127.0.0.1:6379> ttl k1
(integer) 32
127.0.0.1:6379> get k1
"v1keepttl"
127.0.0.1:6379> ttl k1
(integer) 19
127.0.0.1:6379> 
```