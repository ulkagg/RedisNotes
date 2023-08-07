##### 安装
###### 1.在云服务器下载redis-7.0.12.tar.gz
`wget http://download.redis.io/releases/redis-7.0.12.tar.gz`
###### 2.新建opt文件夹进入
`mkdir opt`
`cd opt`
###### 3.Linux pwd cat ll cp命令,快速查找
+ `pwd`
	Linux pwd（英文全拼：print work directory） 命令用于显示工作目录。
	执行 pwd 指令可立刻得知您目前所在的工作目录的绝对路径名称。
+ `cat`
	查看可编辑文件中的内容
+ `ll`
	# linux里ll命令查看文件单位大小
+ `cp`
	复制**目标文件**到**所需文件夹目录下**
	`cp redis.conf /root/opt/myredis/redis7.conf`
	1.进入到**目标文件所在文件夹目录**
	2.执行命令,需要复制的目标文件目录路径名称标全
+ 快速查找
	1.`esc键`
	2.`shift+:`
	3.在命令行按下`/所需要搜索的内容`
###### 4.执行解压命令
`tar -zxvf redis-7.0.12.tar.gz`
此时解压命令会失败
因为目前`pwd`命令可知在Linux `/root/opt`文件夹下面
需要将`windTerm`中`redis-7.0.12.tar.gz`迁移到`/root/opt`命令即可
###### 5.进入已解压的Redis文件夹
`cd redis-7.0.12`
`ll # linux里ll命令查看文件单位大小`
###### 6.编译安装make && make install
###### 7.查看安装目录
默认安装在Linux的`/usr/Local`目录下,相当于windows的`c:\Program Files`
`cd /usr/local/bin`**注意:3个文件夹目录下都有`/`,文件名是usr而不是user**
`ll`查看文件夹内容
###### 8.复制文件Redis.conf
1.在`opt`文件夹下`mkdir myredis`
2.`cd /root/opt/redis-7.0.12`进入文件夹
3.`cp redis.conf /root/opt/myredis/redis7.conf`复制到该文件夹下面
4.Linux下需要进到redis-7.0.12文件夹无root`[root@ulkagg redis-7.0.12]# cp redis.conf /opt/myredis/redis7.conf
`
###### 9.进入编辑
+ 进入步骤
	首先进入到`cd /root/opt/myredis`
	`vim redis7.conf`
	1.按下esc键
	2.同时shift:
	`set nu`
+ 修改内容
	1.作为一个后端服务器`daemonize no`改为`daemonize yes`
	2.`protected-mode` yes改为no
	3.bind 127.0.0.1注释掉,不影响远程ip连接
	4.设置密码:搜索`requirepass foobared`,
		删除前面的#号键
		并在开头添加空格
		删除`foobared`,`空格键`+`密码`
+ `wq!`保存退出
+ 在此我所用的是`windTerm.exe`客户端,可以下载查看是否更改成功
+ 同样直接下载上传一样效果
###### 10.切换配置文件,配置端口启动后台
`redis-server /root/opt/myredis/redis7.conf`
切换配置文件到redis7.conf
`ps -ef|grep redis|grep -v grep`
查看端口是否占用成功,若被占用则redis启动成功
`polkitd   1548  1474  0 10:56 ?        00:00:00 redis-server *:6379`
###### 11.连接服务
`redis-cli -a 926538 -p 6379`
	-a是授权登录-->`redis- cli`+`auth 926538`
新建会话,输入用户与密码:root 926538 `ps -ef|grep redis|`
	想去掉字体颜色的话:`ps -ef|grep redis|grep -v grep`
	显示`polkitd   1548  1474  0 10:56 ?        00:00:00 redis-server *:6379`
	`root      4899  3339  0 11:04 pts/0    00:00:00 redis-cli -a 926538 -p 6379`
返回原来会话窗口`ps -ef|grep redis`响应为`pong`则连接成功
###### 12.退出客户端
`quit`
###### 13.消除连接时显示警告
`Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.`
启用命令`redis-cli -a 926538 -p 6379 2>/dev/null`将错误扔进Linux的黑洞文件
###### 14.Redis的"helloworld"
`set k1 helloworld`
`get k1`
```redis
127.0.0.1:6379> set k1 helloworld
OK
127.0.0.1:6379> get k1
"helloworld"
```
###### 15.Redis的关闭
1.单实例关闭
	```redis
	127.0.0.1:6379> shutdown
	not connected> quit
	```
	这种在服务器127.0.0.1:6379内部,可以直接shutdown然后quit
	<font color="red">新建会话进行关闭?有问题,到其他会话窗口关闭显示密码错误
	`redis-cli -a 926538 shutdown`</font>
	回到主窗口ping会not connected
2.多实例关闭
	指定端口关闭:`redis-cli -p 6379 shutdown`
###### 16.卸载
停止服务
删除/usr/local/bin目录下与redis相关的文件
`ls -l /usr/local/redis- *`
`rm - rf /usr/local/bin/redis- *`
