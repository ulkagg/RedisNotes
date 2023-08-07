## **RDB[#](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/monkey-code/p/13087736.html%234113548194)**

### **简介[#](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/monkey-code/p/13087736.html%233970402563)**

RDB持久化方式是通过快照(snapshotting)完成的，当符合一定条件时，redis会自动将内存中所有数据以二进制方式生成一份==副本==并存储在硬盘上。当redis重启时，并且AOF持久化未开启时，redis会读取RDB持久化生成的二进制文件(默认名称==dump.rdb==，可通过设置dbfilename修改)进行数据恢复，对于持久化信息可以用过命令“info Persistence”查看。
### 版本变更
6版本保存频率较高 15min/1key
7版本保存频率较低 1h/1改变

