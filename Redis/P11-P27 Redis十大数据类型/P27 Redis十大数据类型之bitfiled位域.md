+ 作用
	+ 位域修改:直接对字节码操作修改
	+ 溢出控制
+ 讲一个Redis字符串看做是一个由二进制位组成的数据,并能对变长位宽和任意没有字节对齐的指定整形位域进行寻址和修改
+ 当需要一个模型时,有符号整型需要在位数前加i,无符号在位数前加u
	+ 例如,u8是一个8位无符号整型,i16是一个16位的有符号整型
[ascii码 | ascii码对照表](https://ascii.org.cn/)

### BITFILED key [GET type offset]
+ type:有符号/无符号
+ offset:起始偏移量
+ BITFIELD filedkey get i8 0
	+ i8:取8位
	+ 0:从第0位开始
```sql
127.0.0.1:6379> set filedkey hello
OK
127.0.0.1:6379> BITFIELD filedkey get i8 0
1) (integer) 104<--|0110 1000|0150|104|0x68|h|小写字母h|
127.0.0.1:6379> BITFIELD filedkey get i8 8
1) (integer) 101<--|0110 0101|0145|101|0x65|e|小写字母e|
127.0.0.1:6379> BITFIELD filedkey get i8 16
1) (integer) 108<--|0110 1100|0154|108|0x6C|l|小写字母l|
127.0.0.1:6379> BITFIELD filedkey get i8 24
1) (integer) 108<--|0110 1100|0154|108|0x6C|l|小写字母l|
127.0.0.1:6379> BITFIELD filedkey get i8 32
1) (integer) 111<--|0110 1111|0157|111|0x6F|o|小写字母o|
```
### BITFILED key [SET type offset value] set设置
+ type:有符号/无符号
+ offset:起始偏移量
```sql
127.0.0.1:6379> BITFIELD filedkey set i8 0 120
1) (integer) 104
127.0.0.1:6379> get filedkey
"xello"
```
### BITFILED key [INCRBY type offset increment]自增
+ type:有符号/无符号
+ offset:起始偏移量
```sql
127.0.0.1:6379> BITFIELD filedkey INCRBY u4 2 1
1) (integer) 15
127.0.0.1:6379> BITFIELD filedkey INCRBY u4 2 1
1) (integer) 0

```
### 溢出控制
在Redis中，`BITFIELD`命令允许你对一个字符串类型的值进行位操作。它可以用于创建、设置、获取和更新指定偏移量上的位字段，并且还支持溢出控制。

溢出控制是指在执行`BITFIELD`操作时，如果对于指定的位字段，新的值超出了所设置的位长度范围，应该如何处理。在`BITFIELD`命令中，可以通过指定溢出类型来控制处理行为。

在Redis中，溢出类型有三种：

1. `OVERFLOW WRAP`: 溢出时，将新的值重新回环到字段的最小值。例如，对于一个3位长度的无符号整数(0~7)，如果新值为8，那么使用溢出回环后的结果将是0。
    
2. `OVERFLOW SAT`: 溢出时，将新的值截断为字段的最大值或最小值。对于无符号整数字段来说，截断为最大值意味着新值将变为字段所能表示的最大值；截断为最小值则意味着新值将变为0。
    
3. `OVERFLOW FAIL`: 溢出时，不执行任何修改操作，返回一个错误。
    
```sql
127.0.0.1:6379> get mykey
"a"
127.0.0.1:6379> BITFIELD mykey get i8 0
1) (integer) 97
127.0.0.1:6379> BITFIELD mykey set i8 0 127
1) (integer) 97
127.0.0.1:6379> BITFIELD mykey get i8 0
1) (integer) 127
127.0.0.1:6379> get mykey
"\x7f"

```
+ wrap
```sql
127.0.0.1:6379> BITFIELD mykey set i8 0 138
1) (integer) 127
127.0.0.1:6379> BITFIELD mykey get i8 0
1) (integer) -118
127.0.0.1:6379> get mykey
"\x8a"

```
+ SAT
```sql
127.0.0.1:6379> BITFIELD mykey overflow SAT set i8 0 128
1) (integer) -118
127.0.0.1:6379> BITFIELD mykey get i8 0
1) (integer) 127
127.0.0.1:6379> get mykey
"\x7f"

```
+ fail
```sql
127.0.0.1:6379> BITFIELD mykey overflow fail set i8 0 128
1) (nil)

```