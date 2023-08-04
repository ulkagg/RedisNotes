### XGROUP CREATE
使用 XGROUP CREATE 创建消费者组，语法格式：
`XGROUP [CREATE key groupname id-or-$] [SETID key groupname id-or-$] [DESTROY key groupname] [DELCONSUMER key groupname consumername]`
==被A组的consumer1读取后,不能再被consumer2读取,但是可以被B组的消费者读取==
==不能是阻塞block==
- **key** ：队列名称，如果不存在就创建
- **groupname** ：组名。
- **$** ： 表示从尾部开始消费，只接受新消息，当前 Stream 消息会全部忽略。
+ **>** : 表示第一条尚未消费的消息开始读取
从头开始消费:

XGROUP CREATE mystream consumer-group-name 0-0  

从尾部开始消费:

XGROUP CREATE mystream consumer-group-name $
```sql
127.0.0.1:6379> xgroup create x1 groupA $
OK
127.0.0.1:6379> xgroup create x1 groupB 0
OK
127.0.0.1:6379> xreadgroup group groupA consumer1 streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061685049-0"
         2) 1) "k1"
            2) "v1"
      2) 1) "1691061694109-0"
         2) 1) "k2"
            2) "v2"
            3) "k3"
            4) "v3"
      3) 1) "1691061699717-0"
         2) 1) "k4"
            2) "v4"
      4) 1) "1691061709096-0"
         2) 1) "k5"
            2) "v5"
            3) "k6"
            4) "v6"
      5) 1) "1691061714116-0"
         2) 1) "k7"
            2) "v7"
```
### 负载均衡思想
让组内的多个消费者共同分担读取消息,所以,我们通常会让每个消费者读取部分消息从而实现==消息读取负载在多个消费者之间是均衡分布的==
```sql
127.0.0.1:6379> xreadgroup group groupC consumer1 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061685049-0"
         2) 1) "k1"
            2) "v1"
      2) 1) "1691061694109-0"
         2) 1) "k2"
            2) "v2"
            3) "k3"
            4) "v3"
127.0.0.1:6379> xreadgroup group groupC consumer2 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061699717-0"
         2) 1) "k4"
            2) "v4"
      2) 1) "1691061709096-0"
         2) 1) "k5"
            2) "v5"
            3) "k6"
            4) "v6"
127.0.0.1:6379> xreadgroup group groupC consumer3 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061714116-0"
         2) 1) "k7"
            2) "v7"
```
### XREADGROUP GROUP

使用 XREADGROUP GROUP 读取消费组中的消息，语法格式：

`XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]`

- **group** ：消费组名
- **consumer** ：消费者名。
- **count** ： 读取数量。
- **milliseconds** ： 阻塞毫秒数。
- **key** ： 队列名。
- **ID** ： 消息 ID。

XREADGROUP GROUP consumer-group-name consumer-name COUNT 1 STREAMS mystream >
```sql
127.0.0.1:6379> xreadgroup group groupC consumer1 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061685049-0"
         2) 1) "k1"
            2) "v1"
      2) 1) "1691061694109-0"
         2) 1) "k2"
            2) "v2"
            3) "k3"
            4) "v3"
127.0.0.1:6379> xreadgroup group groupC consumer2 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061699717-0"
         2) 1) "k4"
            2) "v4"
      2) 1) "1691061709096-0"
         2) 1) "k5"
            2) "v5"
            3) "k6"
            4) "v6"
127.0.0.1:6379> xreadgroup group groupC consumer3 Count 2 Streams x1 >
1) 1) "x1"
   2) 1) 1) "1691061714116-0"
         2) 1) "k7"
            2) "v7"
```

### 消息的已读未签收和已读已签收ACK机制
<font color=red>问题:基于Stream实现的消息队列,如何保证消费者在发生故障或者宕机再次重启后,仍然可以读取未处理完的消息?</font>
1. Streams会自动使用内部队列(也叫 PENDING List)留存消费者组里每个消费者读取的消息保底措施,直到消费者使用XACK命令通知Streams "消息已经处理完成"
2. 消息确认增加了消息的可靠性,一般业务处理完成之后,需要执行XACK命令确认消息已被消费完成

![](image-20230803195825140.png)
### XPENDING key groupname 查看每个消费组的所有消费者[已读取,但未确认]的消息 查看某个消费者具体读取了哪些消息

```sql
127.0.0.1:6379> xpending x1 groupC
1) (integer) 5
2) "1691061685049-0"<--所有消费者读取的消息最小ID
3) "1691061714116-0"<--所有消费者读取消息最大UD
4) 1) 1) "consumer1"
      2) "2"<--一口气读两条
   2) 1) "consumer2"
      2) "2"
   3) 1) "consumer3"
      2) "1"
127.0.0.1:6379> XPENDING x1 groupA
1) (integer) 5
2) "1691061685049-0"
3) "1691061714116-0"
4) 1) 1) "consumer1"
      2) "5"
127.0.0.1:6379> XPENDING x1 groupC - + 10 consumer2
1) 1) "1691061699717-0"
   2) "consumer2"
   3) (integer) 1601101
   4) (integer) 1
2) 1) "1691061709096-0"
   2) "consumer2"
   3) (integer) 1601101
   4) (integer) 1
```
一旦消息1691061714116-0被consumer3处理了,consumer3就可以使用XACK命令通知Streams,然后这条消息就会被删除
### XACK 向消息队列确认消息处理已完成
```sql
127.0.0.1:6379> XPENDING x1 groupC - + 10 consumer2
1) 1) "1691061699717-0"
   2) "consumer2"
   3) (integer) 1601101
   4) (integer) 1
2) 1) "1691061709096-0"
   2) "consumer2"
   3) (integer) 1601101
   4) (integer) 1
127.0.0.1:6379> xack x1 groupC 1691061699717-0
(integer) 1<--已读已确认
127.0.0.1:6379> XPENDING x1 groupC - + 10 consumer2
1) 1) "1691061709096-0"
   2) "consumer2"
   3) (integer) 1720913
   4) (integer) 1<--再次签收只有一条未读
```
### XINFO stream [streamname]
打印分组详细信息
```sql
127.0.0.1:6379> XINFO stream x1
 1) "length"
 2) (integer) 4
 3) "radix-tree-keys"
 4) (integer) 1
 5) "radix-tree-nodes"
 6) (integer) 2
 7) "last-generated-id"
 8) "1691122366319-0"
 9) "max-deleted-entry-id"
10) "0-0"
11) "entries-added"
12) (integer) 4
13) "recorded-first-entry-id"
14) "1691122343664-0"
15) "groups"
16) (integer) 1
17) "first-entry"
18) 1) "1691122343664-0"
    2) 1) "v1"
       2) "k1"
       3) "v2"
       4) "k2"
19) "last-entry"
20) 1) "1691122366319-0"
    2) 1) "v6"
       2) "k6"

```
+ 生产使用较少,慎用,建议kafka,rabbitmq