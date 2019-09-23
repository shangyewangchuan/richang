> ## **登陆mysql**

mysql -u 用户名 -p -h 主机 -P 端口

> ## **帮助**

help [命令]

> ## **数据库**

查询数据库列表  
`show databases;`

查询建库语句  
`show create database [database];`

删除数据库  
`drop database [database];`

选择数据库  
`use [databases];`

> ## **数据表**

### **表操作**

查询数据表列表  
`show tables;`

查询建表语句  
`show create table [table];`

查询数据表字段结构  
`show columns from [table];`  
`describe [table]或desc [table];`
`explain [table];`
`show fields from [table];`

创建数据表

create table `[IF NOT EXISTS | IF EXISTS ]` [表](`字段1 数据类型1 [null/not null] [auto_increment/dafault srt]`,字段2 数据类型2 [null/not null] [auto_increment],...,`[primary key(主键字段1,主键字段2,...)]`,`[index/key | unique key | fulltext key] [索引名称](索引字段),CONSTRAINT [约束名称] FOREIGN KEY [外键字段] REFERENCES [外键表名](外键字段，外键字段2…..) ON DELETE CASCADE ON UPDATE CASCADE`)engine=[存储引擎] default character set [字符集] collate [校对];

- 在创建新表时，指定的表名必须不存在，否则将出错。
- 主键自动索引
- auto_increment必须索引，last_insert_id()获取auto_increment值
- 外键不能跨引擎

删除数据表  
`drop table [table];`

#### 修改数据表  

增加/修改字段  
`alter table [表] [add/change] [字段] [数据类型] [参数];`

删除字段  
`alter table [表] drop COLUMN [字段];`

定义外键  
`alter table [表] add constraint [外键名] foreign key (字段) references [外表] [外表字段];`

#### 重命名数据表

重命名单个表  
`rename [原表] to [改表];`

重命名多个表  
`rename [原表1] to [改表1],[原表2] to [改表2],...;`

#### 复杂的表结构手动删除过程

- 用新的列布局创建一个新表
- 使用INSERT SELECT语句从旧表复制数据到新表
- 检验包含所需数据的新表
- 重命名旧表（如果确定，可以删除它）
- 用旧表原来的名字重命名新表
- 根据需要，重新创建触发器、存储过程、索引和外键

### **检索**

测试数据  
`select [操作];`

检索单个列  
`select [表字段] from [table];`

检索多个列  
`select [表字段，表字段] from [table];`

检索所有列  
`select * from [table];`

去除重复检索结果  
`select distinct [表字段] from [table];`

- DISTINCT关键字应用于所有列而不仅是前置它的列。

限制检索行数  
`select [表字段] from [table] limit [number1,number2];`

- 显示number1~number2的检索结果。
- 检索出来的第一行为行0而不是行1。

完全限定表名和列名  
`[database].[table]`  
`[table].[表字段]`

别名|导出列  
`[字段] as [别名]`

### **排序**

根据单个字段排序  
`select [表字段] from [table] order by [表字段];`

- 默认升序(ASC)
- 降序： [表字段] desc

按多个字段排序  
`select [表字段] from [table] order by [表字段1,表字段2];`

- 排序优先级： 表字段1>表字段2

### **过滤**

where子句  
`select [表字段1,表字段2] from [table] where [条件];`

- 条件 = 字段 + where操作符 + 字段值
- 可以通过()控制计算优先级
- 特殊操作符
  - `and`：与
  - `or`： 或
  - `in` (值1,值2)： 匹配每个条件
    - where [字段] in (值1，值2)
    - 相当于where [字段] = 值1 or [字段] = 值2
  - `not`： 取反
    - not in (值1,值2)
    - not between [number1] and [number2]
    - not exists
  - `like`： 使用通配符
    - %通配: 通配任何多个字符
    - _通配: 通配任何单个字符
  - `regexp`：正则匹配
    - \\\：转义
- 特殊条件
  - where [字段] is null
    - 过滤为空的字段
  - where between [number1] and [number2]
    - 过滤范围值

![操作符](https://raw.githubusercontent.com/shangyewangchuan/material/master/img/mysql_sql_where.png)

### **函数**

|函数|作用|实例|
|-|-|-|
|concat()|拼接,用逗号分隔拼接值|-|
|去空格|-|-|
|rtrim()|去掉值右边的所有空格|-|
|ltrim()|去掉值左边的所有空格|-|
|trim()|去掉值左右两边的所有空格|-|
|rtrim()|去掉值右边的所有空格|-|
|大小写|-|-|
|upper()|将文本转换为大写|-|
|lower()|将文本转换为小写|-|
|取值函数|-|-|
|left()|返回值左边的字符|-|
|right()|返回值右边的字符|-|
|length()|返回值的长度|-|
|locate()|返回值的一个子值|-|
|subString()|返回子值的字符|-|
|soundex()|返回值的SOUNDEX值，对值进行发音比较|-|
|时间函数|-|-|
|now()|返回当前日期和时间|-|
|CurDate()|返回当前日期|-|
|CurTime()|返回当前时间|-|
|Date()|返回日期时间的日期部分|-|
|DateDiff()|计算两个日期之差|-|
|Date_Add()|高度灵活的日期运算函数|-|
|Date_Format()|返回一个格式化的日期或时间串|-|
|Day()|返回一个日期的天数部分|-|
|DayOfWeek()|对于一个日期，返回对应的星期几|-|
|Hour()|返回一个时间的小时部分|-|
|Minute()|返回一个时间的分钟部分|-|
|Month()|返回一个日期的月份部分|-|
|Second()|返回一个时间的秒部分|-|
|Time()|返回一个日期时间的时间部分|-|
|Year()|返回一个日期的年份部分|-|
|处理函数|-|-|
|Abs()|返回一个数的绝对值|-|
|Cos()|返回一个角度的余弦|-|
|Exp()|返回一个数的指数值|-|
|Mod()|返回除操作的余数|-|
|Pi()|返回圆周率|-|
|Rand()|返回一个随机数|-|
|Sin()|返回一个角度的正弦|-|
|Sqrt()|返回一个数的平方根|-|
|Tan()|返回一个角度的正切|-|
|聚集函数|-|-|
|AVG()|返回某列的平均值|-|
|COUNT()|返回某列的行数|-|
|MAX()|返回某列的最大值|-|
|MIN()|返回某列的最小值|-|
|SUM()|返回某列值之和|-|

### **分组数据**

#### **数据分组**

select [字段] from [表] `group by` [字段];

GROUP BY子句指示MySQL分组数据，然后对每个组而不是
整个结果集进行聚集。

- 可以包含任意数目的列
- 将在最后规定的分组上进行汇总
- 列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）
- 不能使用别名
- 除聚集计算语句外， SELECT语句中的每个列都必须在GROUP BY子句中给出
- 会统计null值
- 在GROUP BY子句后使用`WITH ROLLUP`关键字可以显示所有分组和汇总值

#### **过滤分组**

having关键字

select [字段] from [表] `group by` [字段] `having` [条件];

- `where`基于行，在分组前过滤；`having`基于分组，在分组后过滤
- `having`支持所有where操作符

### **联结**

?> 联结可以优化多个子查询

##### 等值联结（内部联结、自然联结）

where写法  
`from [表1],[表2] where [条件]`  
规范写法join  
`from [表1] inner join [表2] on [条件]`

##### 自联结

from `[表1]` as `[别名1]`,`[表1]` as `[别名2]` where `[别名1.字段]=[别名2.字段]`

##### 外部联结

额外检索没有关联行的行

左联结：`选择左表所有行`  
`from [表1] LEFT OUTER JOIN [表2] on [条件]`;  
右联结：`选择右表所有行`  
`from [表1] RIGHT OUTER JOIN [表2] on [条件]`;  
多表联结：  
`from [表1] LEFT OUTER JOIN [表2] on [条件1] LEFT OUTER JOIN [表3] on [条件2]`;

*注意*

- 联结必须使用where
- 不使用where进行的联结返回的结果为`笛卡儿积`(叉联结)，即表1的行数乘表2的行数，`第一个表中的每个行将与第二个表中的每个行配对`，没有进行逻辑过滤

### **组合查询**

?> 替代多个where子句

union：将多个语句输出组成单个查询集合

select [字段] from [表] `union [all]` select [字段] from [表];

*注意*

- union自动去重，all显示重复
- UNION中的每个查询必须包含相同的列、表达式或聚集函数
- 列数据类型必须兼容
- 只能使用一条ORDER BY子句

### **全文本搜索**

!> 只有MyISAM支持`全文本搜索`，而且需要建立索引。

?> 相对于通配符和正则表达式，更好性能、容易控制、结果智能化

普通查询  
`where match(索引列) against(表达式)`

扩展查询(搜索相关)  
`where match(索引列) against(表达式 with query expansion)`

布尔查询（不依赖索引）  
`where match(索引列) against(表达式 in boolean mode)`

*布尔查询规则*

- 要匹配的词
- 要排斥的词
- 排列提示
- 分组
- 布尔操作符

![布尔操作符](https://raw.githubusercontent.com/shangyewangchuan/material/master/img/mysql_20190505104907.png)

> ## **SHOW命令**

显示广泛的服务器状态信息

show status;

查询当前用户权限

show grants;

查询服务器错误

show errors;

查询服务器警告

show warnings;

### **流程控制**

case when score >= 60 then 1 else 0 end

### **插入数据**

#### 插入单行

简便插入单行  
`insert into [表] values(字段值1,字段值2，...);`

安全插入单行  
`insert into [表](字段1,字段2,...) values(字段值1,字段值2，...);`

- `insert [LOW PRIORITY] into`，降低insert优先级来提高检索性能

#### 插入多行

方法一：分号分隔   
`insert into;insert into;...`

方法二：逗号分隔    
`insert into [表](字段1,字段2,...) values(字段值1,字段值2，...)，(字段值1,字段值2，...)...;`

- 因为只使用了一个insert语句，性能更好

#### INSERT SELECT

`insert into [插入表](字段1,字段2,...) select 字段值1,字段值2，... from [检索表]);`

- 不一定要求列名匹配,只匹配列的位置

### **更新数据**

!> 在使用UPDATE时一定要注意细心，确定限制条件。因为稍不注意，就会更新表中所有行。

#### 更新单行

`update [表] set [字段]=[字段值] where [条件];`

#### 更新多行

`update [表] set [字段1]=[字段值1],[字段2]=[字段值2],... where [条件];`

- 如果用UPDATE语句更新多行，并且在更新这些行中的一行或多行时出一个现错误，则整个UPDATE操作被取消。
- 即使是发生错误，也继续进行更新，可使用IGNORE关键字
  - `UPDATE IGNORE [表]`

### **删除数据**

!> 在使用DELETE时一定要注意细心，确定限制条件。因为稍不注意，就会更新表中所有行。

#### update删除列数据

`set [字段]=null`

#### delete删除行数据

`delete from [表] where [条件];`

- 如果想从表中删除所有行，不要使用DELETE。可使用TRUNCATE TABLE语句。
- TRUNCATE实际是删除原来的表并重新创建一个表，而不是逐行删除表中的数据。

### **索引**

普通索引(MUL)

- 基本的索引，不具备唯一性，就是加快查询速度
  
`alter table [表] add [index/key] [索引名称](索引字段);`

唯一性索引(UNI)

- 索引列的所有值必须唯一，允许有NULL值

`alter table [表] unique [索引名称](索引字段);`

主键索引(PRI)

- 主键自动索引，不允许为空

复合索引(PRI)

- 复合主键

全文索引  
`alter table [表] add fulltext [索引名](字段);`

外键  
`alter table [表] add constraint [约束名]foreign key (外键字段) references [外键表名](外键字段) on delete cascade on update cascade,type=innodb;`

删除外键  
`alter table [表] drop foreign key [约束名称 | 外键名称]`

> ## **字句顺序**

 `select [from] [where] [group by] [having] [order by] [limit]`

> ## **事务**

### ACID

- 原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

- 一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。

- 隔离性：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。

- 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

### 事务操作

开始事务  
`start transaction;`

- 事务处理用来管理INSERT、 UPDATE和DELETE语句

回退  
`rollback;`

- 不能回退SELECT语句、CREATE或DROP操作

保留点

- 回退部分操作

创建保留点  
`savepoint [保留点名];`

回退保留点  
`rollback to [保留点名];`

释放保留点  
`release savepoint;`

- 保留点在事务处理完成（执行一条ROLLBACK或COMMIT）后自动释放。

提交  
`commit;`

- 事务不会隐式提交，需要显示提交
- 更改默认提交行为,`set autocommit=0;`
- autocommit标志是针对每个连接而不是服务器的

> ## **权限管理**

查看当前用户  
`use mysql;`  
`select user from user;`

创建用户  
`create user [用户名] identified by '密码';`

GRANT
INSERT

重命名用户  
`rename user [原名] to [改名];`  
UPDATE直接更新user表

删除用户和用户权限  
`drop user [用户名];`

- 自MySQL 5以来， DROP USER删除用户账号和所有相关的账号权限。
- 在MySQL 5以前， DROP USER只能用来删除用户账号，不能删除相关的权限。先用REVOKE删除与账号相关的权限，然后再用DROP USER删除账号

查询权限

`show grants for [用户名];`  

设置权限

`grant [权限] on [数据库].[数据表] to [用户名];`

撤销权限  
`revoke [权限] on [数据库].[数据表] to [用户名];`

  <center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://raw.githubusercontent.com/shangyewangchuan/material/master/img/mysql_20190516162604.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">权限表</div>
 </center>

更改密码  
`set password=password('密码');`  
`set password for [用户名] =password(密码);`

> ## **维护**

### 备份

mysqldump
mysqlhotcopy
BACKUP TABLE或SELECT INTO OUTFILE

在进行备份前使用FLUSH TABLES语句刷新数据  

### 日志

错误日志，包含启动和关闭问题以及任意关键错误的细节  
查询日志，记录所有MySQL活动  
二进制日志，记录更新过数据（或者可能更新过数据）的所有
语句  
慢查询日志，记录执行缓慢的任何查询