# MySql

## MySQL概述

### 数据库分类

#### 关系型数据库(SQL)

通过表与表之间，行和列之间的关系进行数据存储，例如：MySql、Oracle、SqlServer、PostgreSQL......

#### 非关系型数据库(NoSql)

NoSQL=not only sql，非关系型数据库，对象存储，通过自身对象的属性来决定，例如：Redis、MongDB

### MySQL安装与卸载

https://blog.csdn.net/bobo553443/article/details/81383194

tips：安装之前要确保卸载干净

## MySQL属性

### 数据库列类型

| 列名      | 介绍                    | 字节    |
| --------- | ----------------------- | ------- |
| tinyint   | 十分小的整数            | 1       |
| smallint  | 较小的整数              | 2       |
| mediumint | 中等大小的整数          | 3       |
| int       | 整数                    | 4       |
| bigint    | 较大的整数              | 8       |
| float     | 浮点数                  | 4       |
| double    | 浮点数                  | 8       |
| decimal   | 字符串形式的浮点数      |         |
| char      | 固定大小的字符串        | 0~255   |
| varchar   | 可变大小的字符串        | 0~65535 |
| tinytext  | 微型文本                | 2^8-1   |
| text      | 文本                    | 2^16-1  |
| date      | 日期YYYY-MM-DD          |         |
| datetime  | 时间YYYY-MM-DD HH:mm:ss |         |
| timestamp | 时间戳                  |         |
| year      | 年份                    |         |

### 数据库字段属性

#### 无符号

只能给数值类型用，选择是否有符号，即是否可以为负数

#### 零填充

不足的位数使用0填充

#### 自增

自动在上一条记录的基础上+1，一般用来设计主键，必须是整数类型，可以设置起始值、步长、是否循环

#### 非空

为空就会报错

#### 默认

设置默认值

### MySQL存储引擎

#### 存储引擎概述

和大多数的数据库不同, MySQL中有一个存储引擎的概念, 针对不同的存储需求可以选择最优的存储引擎。

 存储引擎就是存储数据，建立索引，更新查询数据等等技术的实现方式 。存储引擎是基于表的，而不是基于库的。所以存储引擎也可被称为表类型。

 Oracle，SqlServer等数据库只有一种存储引擎。MySQL提供了插件式的存储引擎架构。所以MySQL存在多种存储引擎，可以根据需要使用相应引擎，或者编写存储引擎。

 MySQL5.0支持的存储引擎包含 ： InnoDB 、MyISAM 、BDB、MEMORY、MERGE、EXAMPLE、NDB Cluster、ARCHIVE、CSV、BLACKHOLE、FEDERATED等，其中InnoDB和BDB提供事务安全表，其他存储引擎是非事务安全表。

通过指令查询当前数据库支持的存储引擎 ：

```sql
show engines
```

创建新表时如果不指定存储引擎，那么系统就会使用默认的存储引擎，MySQL5.5之前的默认存储引擎是MyISAM，5.5之后就改为了InnoDB。

#### 存储引擎对比

| 特点         | InnoDB               | MyISAM   | MEMORY | MERGE | NDB  |
| ------------ | -------------------- | -------- | ------ | ----- | ---- |
| 存储限制     | 64TB                 | 有       | 有     | 没有  | 有   |
| 事务安全     | **支持**             |          |        |       |      |
| 锁机制       | **行锁(适合高并发)** | **表锁** | 表锁   | 表锁  | 行锁 |
| B树索引      | 支持                 | 支持     | 支持   | 支持  | 支持 |
| 哈希索引     |                      |          | 支持   |       |      |
| 全文索引     | 支持(5.6版本之后)    | 支持     |        |       |      |
| 集群索引     | 支持                 |          |        |       |      |
| 数据索引     | 支持                 |          | 支持   |       | 支持 |
| 索引缓存     | 支持                 | 支持     | 支持   | 支持  | 支持 |
| 数据可压缩   |                      | 支持     |        |       |      |
| 空间使用     | 高                   | 低       | N/A    | 低    | 低   |
| 内存使用     | 高                   | 低       | 中等   | 低    | 高   |
| 批量插入速度 | 低                   | 高       | 高     | 高    | 高   |
| 支持外键     | **支持**             |          |        |       |      |

#### InnoDB

InnoDB存储引擎提供了具有提交、回滚、崩溃恢复能力的事务安全。但是对比 MyISAM 的存储引擎，InnoDB 写的处理效率差一些，并且会占用更多的磁盘空间以保留数据和索引。

InnoDB 存储引擎不同于其他存储引擎的特点 ：

- 事务控制
- 外键约束
- 存储方式

####  MyISAM

MyISAM 不支持事务、也不支持外键，其优势是访问的速度快，对事务的完整性没有要求或者以SELECT、INSERT为主的应用基本上都可以使用这个引擎来创建表 。有以下两个比较重要的特点：

- 不支持事务
- 文件存储方式

## MySQL操作

### DDL(定义)

数据定义语言DDL用来创建数据库中的各种对象-----表、视图、
索引、同义词、聚簇等，DDL操作是隐性提交的！不能rollback

```sql
-- 建表
CREATE TABLE [IF NOT EXISTS] tablename (
	'字段名' 列类型 [属性] [索引] [注释],
    ……
    '字段名' 列类型 [属性] [索引] [注释],
    PRIMARY KEY(`id`)
)[表类型] [字符集设置] [注释]
--修改表
ALTER TABLE tablename RENAME AS 新表名
--增加表字段
ALTER TABLE tablename ADD 字段名 列属性
--修改表字段
ALTER TABLE tablename MODIFY 字段名 列属性
--删除表字段
ALTER TABLE tablename DROP 字段名
--删除表
DROP TABLE [IF EXISTS] tablename
```

### DML(操作)

数据操纵语言DML主要有三种形式：

1. 插入：INSERT
2. 更新：UPDATE
3. 删除：DELETE

#### INSERT

```sql
INSERT INTO tablename[(字段1,字段2,字段3)] values('值1','值2','值3')
```

#### UPDATE

```sql
UPDATE tablename SET 字段1='值1',……,字段n='值n' [WHERE 条件] 
```

#### DELETE

```sql
DELETE FROM tablename [WHERE 条件] 
```

#### DELETE和TRUNCATE

相同：都能删除表数据，不会删除表结构

不同：TRUNCATE会设置自增列计数器为初始值，并且不会影响事务

### DQL(查询)

数据查询语言DQL基本结构是由SELECT开头的查询语句

#### SQL的完整语法

```sql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1 [as alias1][,table.field2 [as alias2]][……]]}
FROM table_name [as table_alias]
	[LEFT | RIGHT | INNER JOIN table_name2] --联合查询
	[WHERE ...] -- 指定结果需要满足的条件
	[GROUP BY ...] -- 指定结果按照哪几个字段来分组
	[HAVING ...] -- 过滤分组的记录满足的次要条件
	[ORDER BY ...] -- 指定查询记录排序条件
	[LIMIT {[offset,]row_count | row_countOFFSET offset}]; -- 指定记录从哪条到哪条
```

#### SQL的执行顺序

写SQL的顺序：

![image-20210520161848827](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520161848827.png)

真正执行的顺序：

 随着 Mysql 版本的更新换代，其优化器也在不断的升级，优化器会分析不同执行顺序产生的性能消耗不同而动 态调整执行顺序。下面是经常出现的查询顺序：

![image-20210520161931864](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520161931864.png)

![image-20210520164022838](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520164022838.png)

#### 常见的 Join 查询图

![image-20210520162731111](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520162731111.png)

### DCL(控制)

数据控制语言DCL用来授予或回收访问数据库的某种特权，并控制
数据库操纵事务发生的时间及效果，对数据库实行监视等。如：

1) GRANT：授权。

2) ROLLBACK [WORK] TO [SAVEPOINT]：回退到某一点。
回滚---ROLLBACK
回滚命令使数据库状态回到上次最后提交的状态。其格式为：
SQL>ROLLBACK;

3) COMMIT [WORK]：提交。
  在数据库的插入、删除和修改操作时，只有当事务在提交到数据
库时才算完成。在事务提交前，只有操作数据库的这个人才能有权看
到所做的事情，别人只有在最后提交完成后才可以看到。
提交数据有三种类型：显式提交、隐式提交及自动提交。下面分
别说明这三种类型。

(1) 显式提交
用COMMIT命令直接完成的提交为显式提交。其格式为：
SQL>COMMIT；

(2) 隐式提交
用SQL命令间接完成的提交为隐式提交。这些命令是：
ALTER，AUDIT，COMMENT，CONNECT，CREATE，DISCONNECT，DROP，
EXIT，GRANT，NOAUDIT，QUIT，REVOKE，RENAME。

(3) 自动提交
若把AUTOCOMMIT设置为ON，则在插入、修改、删除语句执行后，
系统将自动进行提交，这就是自动提交。其格式为：
SQL>SET AUTOCOMMIT ON；

## MySQL 常用函数

### 数字函数

| 函数名称        | 作 用                                                      |
| --------------- | ---------------------------------------------------------- |
| ABS             | 求绝对值                                                   |
| SQRT            | 求二次方根                                                 |
| MOD             | 求余数                                                     |
| CEIL 和 CEILING | 两个函数功能相同，都是返回不小于参数的最小整数，即向上取整 |
| FLOOR           | 向下取整，返回值转化为一个BIGINT                           |
| RAND            | 生成一个0~1之间的随机数，传入整数参数是，用来产生重复序列  |
| ROUND           | 对所传参数进行四舍五入                                     |
| SIGN            | 返回参数的符号                                             |
| POW 和 POWER    | 两个函数的功能相同，都是所传参数的次方的结果值             |
| SIN             | 求正弦值                                                   |
| ASIN            | 求反正弦值，与函数 SIN 互为反函数                          |
| COS             | 求余弦值                                                   |
| ACOS            | 求反余弦值，与函数 COS 互为反函数                          |
| TAN             | 求正切值                                                   |
| ATAN            | 求反正切值，与函数 TAN 互为反函数                          |
| COT             | 求余切值                                                   |

### 字符串函数

| 函数名称  | 作 用                                                        |
| --------- | ------------------------------------------------------------ |
| LENGTH    | 计算字符串长度函数，返回字符串的字节长度                     |
| CONCAT    | 合并字符串函数，返回结果为连接参数产生的字符串，参数可以使一个或多个 |
| INSERT    | 替换字符串函数                                               |
| LOWER     | 将字符串中的字母转换为小写                                   |
| UPPER     | 将字符串中的字母转换为大写                                   |
| LEFT      | 从左侧字截取符串，返回字符串左边的若干个字符                 |
| RIGHT     | 从右侧字截取符串，返回字符串右边的若干个字符                 |
| TRIM      | 删除字符串左右两侧的空格                                     |
| REPLACE   | 字符串替换函数，返回替换后的新字符串                         |
| SUBSTRING | 截取字符串，返回从指定位置开始的指定长度的字符换             |
| REVERSE   | 字符串反转（逆序）函数，返回与原始字符串顺序相反的字符串     |

### 日期函数

| 函数名称                | 作 用                                                        |
| ----------------------- | ------------------------------------------------------------ |
| CURDATE 和 CURRENT_DATE | 两个函数作用相同，返回当前系统的日期值                       |
| CURTIME 和 CURRENT_TIME | 两个函数作用相同，返回当前系统的时间值                       |
| NOW 和 SYSDATE          | 两个函数作用相同，返回当前系统的日期和时间值                 |
| MONTH                   | 获取指定日期中的月份                                         |
| MONTHNAME               | 获取指定日期中的月份英文名称                                 |
| DAYNAME                 | 获取指定曰期对应的星期几的英文名称                           |
| DAYOFWEEK               | 获取指定日期对应的一周的索引位置值                           |
| WEEK                    | 获取指定日期是一年中的第几周，返回值的范围是否为 0〜52 或 1〜53 |
| DAYOFYEAR               | 获取指定曰期是一年中的第几天，返回值范围是1~366              |
| DAYOFMONTH              | 获取指定日期是一个月中是第几天，返回值范围是1~31             |
| YEAR                    | 获取年份，返回值范围是 1970〜2069                            |
| TIME_TO_SEC             | 将时间参数转换为秒数                                         |
| SEC_TO_TIME             | 将秒数转换为时间，与TIME_TO_SEC 互为反函数                   |
| DATE_ADD 和 ADDDATE     | 两个函数功能相同，都是向日期添加指定的时间间隔               |
| DATE_SUB 和 SUBDATE     | 两个函数功能相同，都是向日期减去指定的时间间隔               |
| ADDTIME                 | 时间加法运算，在原始时间上添加指定的时间                     |
| SUBTIME                 | 时间减法运算，在原始时间上减去指定的时间                     |
| DATEDIFF                | 获取两个日期之间间隔，返回参数 1 减去参数 2 的值             |
| DATE_FORMAT             | 格式化指定的日期，根据参数返回指定格式的值                   |
| WEEKDAY                 | 获取指定日期在一周内的对应的工作日索引                       |

### 聚合函数

| 函数名称     | 作用                             |
| ------------ | -------------------------------- |
| MAX          | 查询指定列的最大值               |
| MIN          | 查询指定列的最小值               |
| COUNT        | 统计查询结果的行数               |
| SUM          | 求和，返回指定列的总和           |
| AVG          | 求平均值，返回指定列数据的平均值 |
| GROUP_CONCAT | 把分组的数据组合成一列           |

tips:聚合函数会忽略NULL值

### 条件判断函数

| 函数名称       | 作用                                             |
| -------------- | ------------------------------------------------ |
| IF(expr,v1,v2) | 如果表达式expr成立，返回结果v1；否则，返回结果v2 |
| IFNULL(v1,v2)  | 如果v1的值不为NULL，则返回v1，否则返回v2         |

### CASE

```sql
CASE  　　
	WHEN e1 　　
	THEN v1 　　
	WHEN e2 　　
	THEN e2 　　
	... 　　
	ELSE vn 
END
```

CASE表示函数开始，END表示函数结束。如果e1成立，则返回v1,如果e2成立，则返回v2，当全部不成立则返回vn，而当有一个成立之后，后面的就不执行了

```sql
CASE expr 
　　WHEN e1 THEN v1
　　WHEN e1 THEN v1
　　...
　　ELSE vn
END
```

如果表达式expr的值等于e1，返回v1；如果等于e2,则返回e2。否则返回vn

## 事务

### 4个基本要素(ACID)

#### 原子性(Atomicity)

一个事务的多个操作，要么一起成功要么一起失败

#### 一致性(Consistency)

一个事务的多个操作，最后的结果一定是一致（唯一）的

#### 隔离性(Isolation)

多个用户同事操作，不会影响事务的结果，即事务之间是有隔离的关系，不会互相影响

#### 持久性(Durability)

一个事务的多个操作，事务提交后，如果出现断电或者其他自然灾害导致数据库不可用，重启之后数据不会丢失

### 隔离导致的问题

#### 脏读

指一个事务读取了另一个事务未提交的数据

#### 不可重复读

一个事务内多次读取数据，结果不同

#### 虚读(幻读)

一个事务内读取到了别的事务插入的数据，导致前后读取不一致

## 索引

MySQL官方对索引的定义是：索引（index）是帮助MySQL高效获取数据的**数据结构**

![1555902055367](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555902055367.png)

左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址（注意逻辑上相邻的记录在磁盘上也并不是一定物理相邻的）。为了加快Col2的查找，可以维护一个右边所示的二叉查找树，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找快速获取到相应数据。

一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。索引是数据库中用来提高性能的最常用的工具。

https://blog.csdn.net/zhizhengguan/article/details/109193886

### 索引优缺点

优势：

- 提高数据检索的效率，降低数据库的IO成本。
- 数据排序，降低CPU消耗

劣势：

- 虽然索引大大提高了查询速度，同时却会降低更新表的速度

  如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因更新所带来的键值变化后的索引信息。

- 索引本质也是一张表，保存着索引字段和指向实际记录的指针，所以也要占用数据库空间，一般而言，索引表占用的空间是数据表的1.5倍

### 不适合创建索引的情况

- 表记录太少
- 经常增删改的表或者字段
- Where 条件里用不到的字段不创建索引
- 过滤性不好的不适合建索引

### 使用索引的操作

假设 index(a,b,c)；

| Where 语句                                              | 索引是否被使用                             |
| ------------------------------------------------------- | ------------------------------------------ |
| where a = 3                                             | 使用到 a                                   |
| where a = 3 and b = 5                                   | 使用到 a，b                                |
| where a = 3 and b = 5 and c = 4                         | 使用到 a,b,c                               |
| where b = 3 或者 where b = 3 and c = 4 或者 where c = 4 | 没使用，最左原则                           |
| where a = 3 and c = 5                                   | 使用到 a， 但是 c 没使用，b 中间断了       |
| where a = 3 and b > 4 and c = 5                         | 使用到 a 和 b， c 不能用在范围之后，b 断了 |
| where a is null and b is not null                       | is null 支持索引 但是 is not null 不支持   |
| where a <> 3                                            | 不能使用索引                               |
| where abs(a) =3                                         | 不能使用索引                               |
| where a = 3 and b like 'kk%' and c = 4                  | 使用到 a,b,c                               |
| where a = 3 and b like '%kk' and c = 4                  | 只用到 a                                   |
| where a = 3 and b like '%kk%' and c = 4                 | 只用到 a                                   |
| where a = 3 and b like 'k%kk%' and c = 4                | 使用到 a,b,c                               |

### 索引的分类

#### 主键索引

primary key，唯一的标识，主键不可重复，只有一个列作为主键

#### 唯一索引

unique key，避免重复列的出现，唯一索引可以重复，多个列都可以标识为唯一索引

#### 普通索引

key/index，默认的索引，index，key关键字来设置

#### 全文索引

fulltext，在特定的数据库引擎才有，可以在一段文本中快速定位数据

#### 组合索引

多个字段创建的索引，遵循最左原则

### 聚簇索引和非聚簇索引

聚簇索引并不是一种单独的索引类型，而是一种数据存储方式。术语‘聚簇’表示数据行和相邻的键值聚簇的存储 在一起。如下图，左侧的索引就是聚簇索引，因为数据行在磁盘的排列和索引排序保持一致。

![image-20210520211641159](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520211641159.png)

#### 聚簇索引的好处

按照聚簇索引排列顺序，查询显示一定范围数据的时候，由于数据都是紧密相连，数据库不不用从多 个数据块中提取数据，所以节省了大量的 io 操作。

#### 聚簇索引的限制

对于 mysql 数据库目前只有 innodb 数据引擎支持聚簇索引，而 Myisam 并不支持聚簇索引。

由于数据物理存储排序方式只能有一种，所以每个 Mysql 的表只能有一个聚簇索引。一般情况下就是该表的主键。

为了充分利用聚簇索引的聚簇的特性，所以 innodb 表的主键列尽量选用有序的顺序 id，而不建议用无序的 id，比如 uuid 这种。

#### 时间复杂度

同一问题可用不同算法解决，而一个算法的质量优劣将影响到算法乃至程序的效率。算法分析的目的在于选择合适算法和改进算法。

时间复杂度是指执行算法所需要的计算工作量，用大 O 表示记为：O(…)

![image-20210520211835491](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520211835491.png)

![image-20210520212305695](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520212305695.png)

### 索引结构

#### Btree 索引结构

黑马教程：https://www.bilibili.com/video/BV1UQ4y1P7Xr?p=6

**Btree又可以写成B-tree**（B-Tree，并不是B“减”树，横杠为连接符，容易被误导）

BTree又叫多路平衡搜索树，一颗 **m** 叉的BTree特性如下：

- 树中每个节点最多包含m个孩子。
- 除根节点与叶子节点外，每个节点至少有 [ceil(m/2)] 个孩子。
- 若根节点不是叶子节点，则至少有两个孩子。
- 所有的叶子节点都在同一层。
- 每个非叶子节点由 n 个 key 与 n+1 个指针组成，其中 [ceil(m/2)-1] <= n <= m-1

celi()：向上取整，例如celi(2.5)=3

#### 演变过程

以5叉 BTree 为例，key的数量：公式推导 [ceil(m/2)-1] <= n <= m-1。所以 2 <= n <=4 。

当 n>4 时，中间节点分裂到父节点，两边节点分裂。

插入 C N G A H E K Q M F W L T Z D P R X Y S 数据为例。（插入时，按照ABCD..顺序）

1). 插入前4个字母 C N G A

![1555944126588](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944126588.png)

2). 插入H，n>4，中间元素G字母向上分裂到新的节点

![1555944549825](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944549825.png)

3). 插入E，K，Q不需要分裂

![1555944596893](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944596893.png)

4). 插入M，中间元素M字母向上分裂到父节点G

（M 在 K、N中间，BTree最多含有n-1个key，这里即4个key，5个指针）

![1555944652560](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944652560.png)

5). 插入F，W，L，T不需要分裂

![1555944686928](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944686928.png)

6). 插入Z，中间元素T向上分裂到父节点中

（插入Z后是，NQTWZ，把中间T提出来，方便更快的查询，所以不提出Z）

![1555944713486](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944713486.png)

7). 插入D，中间元素D向上分裂到父节点中。然后插入P，R，X，Y不需要分裂

![1555944749984](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944749984.png)

8). 最后插入S（R后面），NPQR节点n>5，中间节点Q向上分裂，但分裂后父节点DGMT的n>5，中间节点M向上分裂

![1555944848294](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1555944848294.png)

到此，该BTREE树就已经构建完成了， BTREE树 和 二叉树 相比， 查询数据的效率更高， 因为对于相同的数据量来说，BTREE的层级结构比二叉树小，因此搜索速度快。

#### 图解

![image-20210520190717898](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/image-20210520190717898.png)

**初始化介绍**

一颗 b 树，浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色 所示），

如磁盘块 1 包含数据项 17 和 35，包含指针 P1、P2、P3， P1 表示小于 17 的磁盘块，P2 表示在 17 和 35 之间的磁盘块，P3 表示大于 35 的磁盘块。 真实的数据存在于叶子节点即 3、5、9、10、13、15、28、29、36、60、75、79、90、99。 非叶子节点只不存储真实的数据，只存储指引搜索方向的数据项，如 17、35 并不真实存在于数据表中

**查找过程**

如果要查找数据项 29，那么首先会把磁盘块 1 由磁盘加载到内存，此时发生一次 IO，在内存中用二分查找确定 29 在 17 和 35 之间，锁定磁盘块 1 的 P2 指针，内存时间因为非常短（相比磁盘的 IO）可以忽略不计，通过磁盘块 1 的 P2 指针的磁盘地址把磁盘块 3 由磁盘加载到内存，发生第二次 IO，29 在 26 和 30 之间，锁定磁盘块 3 的 P2 指 针，通过指针加载磁盘块 8 到内存，发生第三次 IO，同时内存中做二分查找找到 29，结束查询，总计三次 IO。

真实的情况是，3 层的 b+树可以表示上百万的数据，如果上百万的数据查找只需要三次 IO，性能提高将是巨大的， 如果没有索引，每个数据项都要发生一次 IO，那么总共需要百万次的 IO，显然成本非常非常高。

![](img/b-tree.jpg)

#### B+tree 索引结构

![1555906287178](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/00001.jpg)

B+Tree为BTree的变种，B+Tree与BTree的区别为：

1). n叉B+Tree最多含有n个key，而BTree最多含有n-1个key。

2). B+Tree的叶子节点保存所有的key信息，依key大小顺序排列。

3). 所有的非叶子节点都可以看作是key的索引部分。

- B-树的关键字和记录是放在一起的，叶子节点可以看作外部节点，不包含任何信息；B+树的非叶子节点中只有关键字和指向下一个节点的索引，记录只放在叶子节点中。
- 在 B-树中，越靠近根节点的记录查找时间越快，只要找到关键字即可确定记录的存在；而 B+树中每个记录的查找时间基本是一样的，都需要从根节点走到叶子节点，而且在叶子节点中还要再比较关键字。从这个角度看 B- 树的性能好像要比 B+树好，而在实际应用中却是 B+树的性能要好些。因为 B+树的非叶子节点不存放实际的数据， 这样每个节点可容纳的元素个数比 B-树多，树高比 B-树小，这样带来的好处是减少磁盘访问次数。尽管 B+树找到 一个记录所需的比较次数要比 B-树多，但是一次磁盘访问的时间相当于成百上千次内存比较的时间，因此实际中 B+树的性能可能还会好些，而且 B+树的叶子节点使用指针连接在一起，方便顺序遍历（例如查看一个目录下的所有 文件，一个表中的所有记录等），这也是很多数据库和文件系统使用 B+树的缘故。

**为什么B+树比 B-树更适合索引？**

- B+树的磁盘读写代价更低

  B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对 B 树更小。如果把所有同一内部结点 的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就 越多。相对来说 IO 读写次数也就降低了。

- B+树的查询效率更加稳定

  由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须 走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

#### MySQL中的B+Tree

MySql索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能。

MySQL中的 B+Tree 索引结构示意图:

![](img/b+tree.jpg)

## SQL优化

sql调优的原则就是能利用索引就利用索引，然后避免全表扫描

### explain分析执行计划

通过 EXPLAIN 或者 DESC命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。

```sql
explain  select * from tablename where id = 1;
```

| 字段          | 含义                                                         |
| ------------- | ------------------------------------------------------------ |
| id            | select查询的序列号，是一组数字，表示的是查询中执行select子句或者是操作表的顺序。 |
| select_type   | 表示 SELECT 的类型，常见的取值有 SIMPLE（简单表，即不使用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION 中的第二个或者后面的查询语句）、SUBQUERY（子查询中的第一个 SELECT）等 |
| table         | 输出结果集的表                                               |
| type          | 表示表的连接类型，性能由好到差的连接类型为( system ---> const -----> eq_ref ------> ref -------> ref_or_null----> index_merge ---> index_subquery -----> range -----> index ------> all ) |
| possible_keys | 表示查询时，可能使用的索引                                   |
| key           | 表示实际使用的索引                                           |
| key_len       | 索引字段的长度                                               |
| rows          | 扫描行的数量                                                 |
| extra         | 执行情况的说明和描述                                         |

#### explain的type解析

type 显示的是访问类型，是较为重要的一个指标，可取值为：

| type   | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| NULL   | MySQL不访问任何表，索引，直接返回结果                        |
| system | 表只有一行记录(等于系统表)，这是const类型的特例，一般不会出现 |
| const  | 表示通过索引一次就找到了，const 用于比较primary key 或者 unique 索引。因为只匹配一行数据，所以很快。如将主键置于where列表中，MySQL 就能将该查询转换为一个常量。const于将 "主键" 或 "唯一" 索引的所有部分与常量值进行比较 |
| eq_ref | 类似ref，区别在于使用的是唯一索引，使用主键的关联查询，关联查询出的记录只有一条。常见于主键或唯一索引扫描 |
| ref    | 非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，返回所有匹配某个单独值的所有行（多个） |
| range  | 只检索给定返回的行，使用一个索引来选择行。 where 之后出现 between ， < , > , in 等操作。 |
| index  | index 与 ALL的区别为 index 类型只是遍历了索引树， 通常比ALL 快， ALL 是遍历数据文件。 |
| all    | 将遍历全表以找到匹配的行                                     |

结果值从最好到最坏以此是：

- NULL > system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL
- system > const > eq_ref > ref > range > index > ALL

一般来说， 我们需要保证查询至少达到 range 级别， 最好达到ref

### 插入大量数据

1. 如果需要同时对一张表插入很多行数据时，应该尽量使用多个值表的insert语句，这种方式将大大的缩减客户端与数据库之间的连接、关闭等消耗。使得效率比分开执行的单个insert语句快
2. 在事务中进行数据插入
3. 数据有序插入

```sql
-- 优化前
insert into tb_test values(2,'Tom');
insert into tb_test values(1,'Cat');
……
insert into tb_test values(9999999,'Jerry');
-- 优化后
start transaction;
insert into tb_test values(1,'Cat'),(2,'Tom')，……(9999999,'Jerry');
commit;
```

### 优化order by语句

- 不是通过索引直接返回排序结果的排序都叫 FileSort 排序
- 通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高
- 尽量减少额外的排序，通过索引直接返回有序数据。where 条件和Order by 使用相同的索引，并且Order By 的顺序和索引顺序相同， 并且Order by 的字段都是升序，或者都是降序。否则肯定需要额外的操作，这样就会出现 FileSort

**Filesort 的优化**

通过创建合适的索引，能够减少 Filesort 的出现，但是在某些情况下，条件限制不能让 Filesort 消失，那就需要加快 Filesort的排序操作。对于Filesort ，MySQL 有两种排序算法：

1） 两次扫描算法 ：MySQL4.1 之前，使用该方式排序。首先根据条件取出排序字段和行指针信息，然后在排序区 sort buffer 中排序，如果sort buffer不够，则在临时表 temporary table 中存储排序结果。完成排序之后，再根据行指针回表读取记录，该操作可能会导致大量随机I/O操作。

2）一次扫描算法：一次性取出满足条件的所有字段，然后在排序区 sort buffer 中排序后直接输出结果集。排序时内存开销较大，但是排序效率比两次扫描算法要高。

MySQL 通过比较系统变量 max_length_for_sort_data 的大小和 Query 语句取出的字段总大小， 来判定是否那种排序算法，如果 max_length_for_sort_data 更大，那么使用第二种优化之后的算法；否则使用第一种。

可以适当提高 sort_buffer_size 和 max_length_for_sort_data 系统变量，来增大排序区的大小，提高排序的效率。

### 优化group by语句

由于 GROUP BY 实际上也同样会进行排序操作，而且与 ORDER BY 相比，GROUP BY 主要只是多了排序之后的分组操作。当然，如果在分组的时候还使用了其他的一些聚合函数，那么还需要一些聚合函数的计算。所以，在GROUP BY 的实现过程中，与 ORDER BY 一样也可以利用到索引。

如果查询包含 group by 但是想要避免排序结果的消耗， 则可以执行order by null 禁止排序

```sql
-- age没有索引
-- 优化前 
select age,count(*) from emp group by age;
-- 优化后
select age,count(*) from emp group by age order by null;
```

### 优化嵌套查询

使用子查询可以一次性的完成很多逻辑上需要多个步骤才能完成的SQL操作，同时也可以避免事务或者表锁死，并且写起来也很容易。但是，有些情况下，子查询是可以被更高效的连接（JOIN）替代。

示例 ，查找有角色的所有的用户信息 :

```sql
select * from t_user where id in (select user_id from user_role );
```

优化后 :

```sql
select * from t_user u , user_role ur where u.id = ur.user_id;
```

连接(Join)查询之所以更有效率一些 ，是因为MySQL不需要在内存中创建临时表来完成这个逻辑上需要两个步骤的查询工作。

### 优化OR条件

对于包含OR的查询子句，如果要利用索引，则OR之间的每个条件列都必须用到索引， 而且不能使用到复合索引； 如果没有索引，则应该考虑增加索引。

建议使用 union 替换 or ：

```sql
select * from emp where id = 1 union select * from emp where id = 30;
```

UNION 语句的 type 值为 ref，OR 语句的 type 值为 range，可以看到这是一个很明显的差距

UNION 语句的 ref 值为 const，OR 语句的 type 值为 null，const 表示是常量值引用，非常快

这两项的差距就说明了 UNION 要优于 OR 。

在 mysql 8.0 中，默认优化了，具体自行测试。

### 优化分页查询

一般分页查询时，通过创建覆盖索引能够比较好地提高性能。一个常见又非常头疼的问题就是 limit 2000000,10 ，此时需要MySQL排序前2000010 记录，仅仅返回2000000 - 2000010 的记录，其他记录丢弃，查询排序的代价非常大 

```sql
select * from tablename limit 2000000,10;
```

**优化思路一**

在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容

```sql
select * from tablename t,(select id from tablename order by id limit 2000000,10) a where t.id = a.id;
```

**优化思路二**

该方案适用于主键自增的表，可以把Limit 查询转换成某个位置的查询 

```sql
select * from tablename where id > 2000000 limit 10;
```



## 使用SQL提示

SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

**USE INDEX**

在查询语句中表名的后面，添加 use index 来提供希望MySQL去参考的索引列表，就可以让MySQL不再考虑其他可用的索引。

```text
create index idx_seller_name on tb_seller(name);
```

1

![1556370971576](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1556370971576.png)

**IGNORE INDEX**

如果用户只是单纯的想让MySQL忽略一个或者多个索引，则可以使用 ignore index 作为 hint 。

```text
 explain select * from tb_seller ignore index(idx_seller_name) where name = '小米科技';
```

1

![1556371004594](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1556371004594.png)

**FORCE INDEX**

为强制MySQL使用一个特定的索引，可在查询中使用 force index 作为hint 。

```sql
create index idx_seller_address on tb_seller(address);
```

1

![1556371355788](https://cdn.jsdelivr.net/gh/oddfar/static/img/MySQL%E9%AB%98%E7%BA%A7.assets/1556371355788.png)

## 数据库设计规范

### 三大范式

#### 第一范式

1NF：要求数据库的表的每一列都是不可分割的原子数据项，即数据的原子性

#### 第二范式

2NF：在1NF的基础上，一个数据库表只描述一件事情

#### 第三范式

3NF：在2NF的基础上，数据库表中的每一列数据都和主键直接相关，而不能间接相关

## JDBC

```java
// 加载驱动
Class.forName("com.mysql.jdbc.Driver");
// 连接数据库
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";
String username = "root";
String password = "123456";
Connection connection = DriverManager.getConnection(url,username,password);
// 操作数据库
Statement statement = connection.createStatement();
String sql = "XXXX";
ResultSet resultSet = statement.executeQuery(sql);
ResultSet resultSet = statement.executeUpdate(sql);
connection.commit();
// 释放连接
resultSet.close();
statement.close();
connection.close();
```

### 数据库连接池

有很多数据库连接池：c3p0、druid

和线程池用法类似，主要目的是为了减少连接和关闭数据库连接所带来的性能消耗，简化操作

## SQL注入

登录输入账号和密码时，密码输一个 **'' or 1=1**，如果后端使用字符串拼接的方式的话就会把输入的**'' or 1=1**拼接去执行，这样就可能登录成功，防止SQL注入的方式就是不要使用字符串拼接，使用占位符，并且把里面的特殊符号转义