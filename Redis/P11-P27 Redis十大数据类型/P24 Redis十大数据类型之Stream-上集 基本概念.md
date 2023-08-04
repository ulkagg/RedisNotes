### 基本理论
Redis Stream 是 Redis 5.0 版本新增加的数据结构。

Redis Stream ==主要用于消息队列==（MQ，Message Queue），Redis 本身是有一个 Redis 发布订阅 (pub/sub) 来实现消息队列的功能，但它有个缺点就是==消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃==。

简单来说发布订阅 (pub/sub) 可以分发消息，但无法记录历史消息。

而 ==Redis Stream 提供了消息的持久化和主备复制功能==，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。

每个 Stream 都有唯一的名称，它就是 Redis 的 key，在我们首次使用 xadd 指令追加消息时自动创建。
![](image-20230802182252023.png)


上图解析：

- **Consumer Group** ：==消费组==，使用 XGROUP CREATE 命令创建，一个消费组有多个消费者(Consumer)。
- **last_delivered_id** ：游标，每个消费组会有个游标 last_delivered_id，任意一个==消费者读取了消息==都会使游标 ==last_delivered_id 往前移动。==
- **pending_ids** ：消费者(Consumer)的状态变量，作用是维护消费者的未确认的 id。 pending_ids 记录了==当前已经被客户端读取的消息==，但是还==没有 ack== (Acknowledge character：确认字符）。
- Message content : 消息内容.

- **start** ：开始值， - 表示最小值
- **end** ：结束值， + 表示最大值
- **$** ： 表示从尾部开始消费，只接受新消息，当前 Stream 消息会全部忽略。
	- 只消费新的消息,当前流中最大的id,可用于将要到来的消息
- `*`  : 用于XADD命令中,让系统自动生成id
- `>` : 用于XREADGROUP命令,表示迄今还没用发送给组中使用者的信息,会更新消费者组的最后ID
