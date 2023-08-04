### XADD

使用 XADD 向队列添加消息，如果指定的队列不存在，则创建一个队列，XADD 语法格式：

XADD key ID field value [field value ...]

- **key** ：队列名称，如果不存在就创建
- **ID** ：消息 id，我们使用 * 表示由 redis 生成，可以自定义，但是要自己保证递增性。
- **field value** ： 记录。
```sql
127.0.0.1:6379> XADD x1 * filed1 value1 filed2 value2 filed3 value3
"1690973719875-0"
减号之前的叫做毫秒级的时间戳,同一毫秒产生的第一条消息
127.0.0.1:6379> XADD x1 1691056739454-0 filed1 value1 filed2 value2 filed3 value3
(error) ERR The ID specified in XADD is equal or smaller than the target stream top item
↑同一时间戳会报错
```
### XLEN

使用 XLEN 获取流包含的元素数量，即消息长度，语法格式：

XLEN key

- **key**：队列名称
```sql
127.0.0.1:6379> XLen x1
(integer) 1
```
### XRANGE

使用 XRANGE 获取消息列表，会自动过滤已经删除的消息 ，语法格式：

XRANGE key start end [COUNT count]

- **key** ：队列名
- **start** ：开始值， - 表示最小值
- **end** ：结束值， + 表示最大值
- **count** ：数量
```sql
1.无count
127.0.0.1:6379> XRANGE x1 - +
1) 1) "1691056739454-0"
   2) 1) "filed1"
      2) "value1"
      3) "filed2"
      4) "value2"
      5) "filed3"
      6) "value3"
2) 1) "1691057104782-0"
   2) 1) "id"
      2) "11"
      3) "cname"
      4) "z3"
3) 1) "1691057117743-0"
   2) 1) "id"
      2) "12"
      3) "cname"
      4) "li4"
```
```sql 
2.有count
127.0.0.1:6379> XRANGE x1 - + count 1
1) 1) "1691056739454-0"
   2) 1) "filed1"
      2) "value1"
      3) "filed2"
      4) "value2"
      5) "filed3"
      6) "value3"
```
### XREVRANGE
### XDEL 依据主键删除
依据主键删除
```sql
127.0.0.1:6379> xdel x1 1691057117743-0
(integer) 1
127.0.0.1:6379> XRANGE x1 - + 
1) 1) "1691056739454-0"
   2) 1) "filed1"
      2) "value1"
      3) "filed2"
      4) "value2"
      5) "filed3"
      6) "value3"
2) 1) "1691057104782-0"
   2) 1) "id"
      2) "11"
      3) "cname"
      4) "z3"
```

### XTRIM 使用 XTRIM 对流进行修剪，限制长度
最小的时间戳将会被截取掉
语法格式：

XTRIM key MAXLEN [~] count

- **key** ：队列名称
- **MAXLEN** ：长度
- **count** ：数量
#### MAXLEN截取
```sql
127.0.0.1:6379> XRANGE x1 - +
1) 1) "1691056739454-0"
   2) 1) "filed1"
      2) "value1"
      3) "filed2"
      4) "value2"
      5) "filed3"
      6) "value3"
2) 1) "1691057104782-0"
   2) 1) "id"
      2) "11"
      3) "cname"
      4) "z3"
3) 1) "1691057892018-0"
   2) 1) "id"
      2) "12"
      3) "cname"
      4) "li4"
127.0.0.1:6379> xtrim x1 maxlen 2
(integer) 1
127.0.0.1:6379> XRANGE x1 - +
1) 1) "1691057104782-0"
   2) 1) "id"
      2) "11"
      3) "cname"
      4) "z3"
2) 1) "1691057892018-0"
   2) 1) "id"
      2) "12"
      3) "cname"
      4) "li4
```
#### MINID截取
```sql
127.0.0.1:6379> xtrim x1 minid 1691057892018-0
(integer) 1
127.0.0.1:6379> XRANGE x1 - +
1) 1) "1691057892018-0"
   2) 1) "id"
      2) "12"
      3) "cname"
      4) "li4"
      5) 
```
### XREAD

使用 XREAD 以阻塞或非阻塞方式获取消息列表 ，语法格式：

XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] id [id ...]

- **count** ：数量
- **milliseconds** ：可选，阻塞毫秒数，没有设置就是非阻塞模式
- **key** ：队列名
- **id** ：消息 ID
#### 无 block阻塞
```sql
1.全部读取
127.0.0.1:6379> xread streams x1 0-0
1) 1) "x1"
   2) 1) 1) "1691058467090-0"
         2) 1) "k1"
            2) "v1"
      2) 1) "1691058476835-0"
         2) 1) "k2"
            2) "v2"
            3) "k3"
            4) "v3"
      3) 1) "1691058483694-0"
         2) 1) "k4"
            2) "v4"
      4) 1) "1691058497459-0"
         2) 1) "k5"
            2) "v5"
            3) "k6"
            4) "v6"
2.读取两条
127.0.0.1:6379> xread count 2 streams x1 0-0
1) 1) "x1"
   2) 1) 1) "1691058467090-0"
         2) 1) "k1"
            2) "v1"
      2) 1) "1691058476835-0"
         2) 1) "k2"
            2) "v2"
            3) "k3"
            4) "v3"
```
#### 有阻塞block
```sql
127.0.0.1:6379> xread count 1 block 0 streams x1 $
新开客户端2
127.0.0.1:6379> xadd x1 * k9 v9
"1691059201774-0"
127.0.0.1:6379> xread count 1 block 0 streams x1 $
1) 1) "x1"
   2) 1) 1) "1691059201774-0"
         2) 1) "k9"
            2) "v9"
(28.03s)
```
