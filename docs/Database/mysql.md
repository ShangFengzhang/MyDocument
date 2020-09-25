## Mysql


<h3>列的数据类型</h3>
>数值

- tinyint  1个字节
- smallint 2个字节
- mediumint     3个字节
- int      4个字节
- bigint   8个字节
- float 4个字节
- double 8个字节
- decimal 字符串形式浮点数 金融计算的时候，一般使用decimal


>字符串

- char 固定大小字符串 0-255
- varchar 可变字符串 0-65535
- tinytext 微型文本 2^8-1
- text 文本串 2^16-1

>时间和日期

- date 日期
- time 时间
- datetime 日期时间
- timestamp 时间戳 1970.1.1到现在的毫秒数
- year 年份

>null

- 没有值
- 不要使用null进行运算

<h3>数据库的字段属性</h3>
Unsigned:   

- 无符号的整数  
- 该列不能为负数

zerofill:

- 0填充的
- 不足的位数使用0来填充

自增:
	
- 自动在上一条记录的基础上+1
- 通常用来设计唯一主键，整数类型
- 设置主键自增的起始值

非空

- 假设设置为not null，如果不赋值就会报错
- 不填写值，默认为null

默认:

- 设置默认值







