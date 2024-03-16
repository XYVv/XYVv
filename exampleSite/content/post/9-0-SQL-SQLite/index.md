+++
author = "XinYu"
title = "SQL——SQLite语句与编程
date = "2024-3-16"
description = "SQLite专题之SQLite语句与编程篇"
categories = [
    "C语言"
]
tags = [
    "SQL","SQLite语句与编程"
]

+++
![](1.jpg)





[TOC]

# SQL基础

## SQLite 数据库简介

SQLite 是一个开源的、 内嵌式的关系型数据库， 第一个版本诞生于 2000 年 5 月， 目前最高版本为 SQLite3。

下载地址： [https://www.sqlite.org/download.html](https://www.sqlite.org/download.html)

菜鸟教程 : [https://www.runoob.com/sqlite/sqlite-tutorial.html](https://www.runoob.com/sqlite/sqlite-tutorial.html)

Linux 下 字符界面

```bash
sudo apt-get install sqlite3
```

Linux 下 图形界面

```bash
sudo apt-get install sqlitebrowser
```

该教程没有使用这个, 因为我下载时找不到

```bash
sudo apt-get install sqliteman
```

SQLite 特性：

* 零配置
* 灵活
* 可移植
* 自由的授权
* 紧凑
* 可靠
* 简单
* 易用  

## SQL 语句基础

SQL 是一种结构化查询语言（Structured Query Language） 的缩写， SQL 是一种专门用来与数据库通信的语言。

SQL 目前已成为应用最广的数据库语言。

SQL 已经被众多商用数据库管理系统产品所采用， 不同的数据库管理系统在其实践过程中都对 SQL 规范作了某些编改和扩充。 故不同数据库管理系统之间的 SQL 语言不能完全相互通用。

SQLite 数据类型 :

一般数据采用固定的静态数据类型， 而 SQLite 采用的是动态数据类型， 会根据存入值自动判断。

SQLite 具有以下五种基本数据类型 ：

* `integer`： 带符号的整型（最多 64 位） 。
* `real`： 8 字节表示的浮点类型。
* `text`： 字符类型， 支持多种编码（如 UTF-8、 UTF-16） ， 大小无限制。
* `blob`： 任意类型的数据， 大小无限制。
* `BLOB`\(binary large object\)二进制大对象， 使用二进制保存数据  
* `null`： 表示空值  

对数据库文件 SQL 语句：

创建、 打开数据库：

当\*.db 文件不存在时， sqlite 会创建并打开数据库文件。

当\*.db 文件存在时， sqlite 会打开数据库文件。

```text
sqlite3 test.db
```

![image-20200809124512574](https://gitee.com/cpu_code/picture_bed/raw/master//20200809124512.png)

SQL 的语句格式：

所有的 SQL 语句都是以分号结尾的， SQL 语句不区分大小写。 两个减号“--” 则代表注释。

关系数据库的核心操作：

* 创建、 修改、 删除表  
* 添加、 修改、 删除行  
* 查表  

### 创建表： create 语句

语法：

```text
create table 表名称 (列名称1 数据类型, 列名称2 数据类型, 列名称3 数据类型, ...);
```

创建一表格该表包含 3 列， 列名分别是： “id” 、 “name” 、 “addr” 。

```text
create table cpucode (id integer, name text, addr text);
```

![image-20200809125437496](https://gitee.com/cpu_code/picture_bed/raw/master//20200809125437.png)

### 创建表： create 语句 \(设置主键\)

在用 sqlite 设计表时， 每个表都可以通过 `primary key` 手动设置**主键,** **每个表只能有一个主键**， 设置为主键的列数据不可以重复。

语法：

```text
create table 表名称 ( 列名称1 数据类型 primary key, 列名称2 数据类型,列名称3 数据类型, ...);
```

```text
create table test (id integer primary key, name text, addr text);
```

![image-20200809124801066](https://gitee.com/cpu_code/picture_bed/raw/master//20200809124801.png)

### 查看表： .table

查看数据表的结构：

```text
.schema[表名]
```

```text
.tables
```

![image-20200809124825297](https://gitee.com/cpu_code/picture_bed/raw/master//20200809124825.png)

```text
.schema
```

![image-20200809125503551](https://gitee.com/cpu_code/picture_bed/raw/master//20200809125503.png)

### 退出数据库命令

```text
.quit
```

```text
.exit
```

![image-20200809125554172](https://gitee.com/cpu_code/picture_bed/raw/master//20200809125554.png)

图形化的软件查看表的结构 :

```text
sqlitebrowser test.db
```

![image-20200809125700156](https://gitee.com/cpu_code/picture_bed/raw/master//20200809125700.png)

![image-20200809125806604](https://gitee.com/cpu_code/picture_bed/raw/master//20200809125806.png)

### 修改表： alter 语句

在已有的表中**添加**或**删除列**以及**修改表名**。\(添加、 删除-sqlite3 暂不支持、 重命名\)

语法 :

```text
alter table 表名 add 列名 数据类型;
```

```text
alter table cpucode add sex text;
```

![image-20200809130120229](https://gitee.com/cpu_code/picture_bed/raw/master//20200809130120.png)

语法： （alter 修改表名）

```text
alter table 表名 rename to 新表名;
```

```text
.tables
```

```text
alter table cpucode rename to new_cpucode;
```

```text
.tables
```

![image-20200809130242617](https://gitee.com/cpu_code/picture_bed/raw/master//20200809130242.png)

### 删除表： drop table 语句

用于删除表（表的结构、 属性以及表的索引也会被删除）

语法：

```text
drop table 表名称;
```

```text
drop table new_cpucode;
```

![image-20200809131750952](https://gitee.com/cpu_code/picture_bed/raw/master//20200809131751.png)

### 插入新行： insert into 语句\(全部赋值\)

给一行中的所有列赋值。

当列值为字符串时要加上`‘ ’` 号。

语法：

```text
insert into 表名 values (列值 1, 列值 2, 列值 3， 列值 4, ...);
```

```text
create table cpucode (id integer, name text, addr text);
```

![image-20200809132249261](https://gitee.com/cpu_code/picture_bed/raw/master//20200809132249.png)

```text
insert into cpucode values (1, 'code', 'changsha');
```

![image-20200809132551917](https://gitee.com/cpu_code/picture_bed/raw/master//20200809132551.png)

```bash
sqlitebrowser test.db
```

![image-20200809132524349](https://gitee.com/cpu_code/picture_bed/raw/master//20200809132524.png)

![image-20200809132446540](https://gitee.com/cpu_code/picture_bed/raw/master//20200809132446.png)

### 插入新行： insert into 语句部分赋值\)

给一行中的部分列赋值

语法：

```text
insert into 表名 (列名 1, 列名 2, ...) values (列值 1, 列值 2, ...);
```

```text
insert into cpucode (id, name) values (1, 'cpu');
```

![image-20200809133320623](https://gitee.com/cpu_code/picture_bed/raw/master//20200809133320.png)

```bash
sqlitebrowser test.db
```

![image-20200809133413766](https://gitee.com/cpu_code/picture_bed/raw/master//20200809133413.png)

![image-20200809133347328](https://gitee.com/cpu_code/picture_bed/raw/master//20200809133347.png)

### 修改表中的数据： update 语句

使用 where 根据匹配条件， 查找一行或多行， 根据查找的结果修改表中相应行的列值\(修改哪一列由列名指定\)。

语法：

```text
update 表名 set 列 1 = 值1 [, 列2 = 值2, ...] [匹配条件];
```

匹配： where 子句

where 子句用于规定匹配的条件。

|  操作数  |   描述   |
| :------: | :------: |
|    =     |   等于   |
| &lt;&gt; |  不等于  |
|   &gt;   |   大于   |
|   &lt;   |   小于   |
|  &gt;=   | 大于等于 |
|  &lt;=   | 小于等于 |

匹配条件语法：

```text
where 列名 操作符 列值
```

```text
update cpucode set id=2, addr='shenzhen' where name='cpu';
```

![image-20200809140409888](https://gitee.com/cpu_code/picture_bed/raw/master//20200809140409.png)

```bash
sqlitebrowser test.db
```

![image-20200809140353950](https://gitee.com/cpu_code/picture_bed/raw/master//20200809140354.png)

当表中有多列、 多行符合匹配条件时会修改相应的多行。 当匹配条件为空时则匹配所有。

![image-20200809140325506](https://gitee.com/cpu_code/picture_bed/raw/master//20200809140325.png)

当表中有多列、 多行符合匹配条件时会修改相应的多行 :

查询

```text
select * from cpucode;
```

![image-20200810095855079](https://gitee.com/cpu_code/picture_bed/raw/master//20200810095855.png)

插入

```text
insert into cpucode values (3, 'test', 'changsha');
```

查询

```text
select * from cpucode;
```

![image-20200810095748022](https://gitee.com/cpu_code/picture_bed/raw/master//20200810095748.png)

修改

```text
update cpucode set name='cpu' where addr='changsha';
```

![image-20200810111341800](https://gitee.com/cpu_code/picture_bed/raw/master//20200810111342.png)

查看

```text
select * from cpucode;
```

当匹配条件为空时则匹配所有 :

修改 :

```text
update cpucode set addr='shenzhen';
```

![image-20200810111523513](https://gitee.com/cpu_code/picture_bed/raw/master//20200810111523.png)

### 删除表中的数据： delete 语句

使用 where 根据匹配条件， 查找一行或多行， 根据查找的结果删除表中的查找到的行。

当表中有多列、 多行符合匹配条件时会删除相应的多行。

语法：

```text
delete from 表名 [匹配条件];
```

删除

```bash
delete from cpucode where name='cpu';
```

查看

```text
select * from cpucode;
```

![image-20200810112210025](https://gitee.com/cpu_code/picture_bed/raw/master//20200810112210.png)

```text
insert into cpucode values (1, 'code', 'changsha');
```

```text
insert into cpucode values (2, 'cpu', 'shenzhen');
```

```text
insert into cpucode values (3, 'test', 'beijing');
```

![image-20200810112452676](https://gitee.com/cpu_code/picture_bed/raw/master//20200810112452.png)

### 查询： select 语句

用于从表中选取数据， 结果被存储在一个结果表中（称为结果集） 。

星号（\*） 是选取所有列的通配符

语法：

```text
 select * from 表名 [匹配条件];
```

```text
select 列名 1[, 列名 2, ...] from 表名 [匹配条件];
```

```text
select * from cpucode
```

![image-20200810112826421](https://gitee.com/cpu_code/picture_bed/raw/master//20200810112826.png)

查看

```text
select * from cpucode where id=2;
```

![image-20200810113213821](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113214.png)

```text
select name from cpucode;
```

![image-20200810113232766](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113232.png)

```text
select name from cpucode where id = 1;
```

![image-20200810113321806](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113321.png)

列名显示

```text
.headers on
```

左对齐

```text
.mode column
```

```text
select * from cpucode where id = 3;
```

![image-20200810113454155](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113454.png)

### 匹配条件语法

数据库提供了丰富的操作符配合 where 子句实现了多种多样的匹配方法。

* in 操作符
* and 操作符
* or 操作符
* between and 操作符
* like 操作符
* not 操作符  

`in` 允许我们在 where 子句中规定多个值。

匹配条件语法：

```text
where 列名 in (列值 1, 列值 2, ...)
```

```text
select * from 表名 where 列名 in (值 1, 值 2, ...);
```

```text
select 列名 1[,列名 2,...] from 表名 where 列名 in (列值 1, 列值 2, ...);
```

```text
select * from cpucode where id in (1, 2);
```

![image-20200810113901131](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113901.png)

```text
select name from cpucode where id in(2, 3);
```

![image-20200810113915934](https://gitee.com/cpu_code/picture_bed/raw/master//20200810113916.png)

`and` 可在 where 子语句中把两个或多个条件结合起来（多个条件之间是与的关系） 。

匹配条件语法：

```text
where 列 1 = 值 1 [and 列 2 = 值 2 and ...]
```

```text
select * from 表名 where 列 1 = 值 1 [and 列 2 = 值 2 and ...];
```

```text
select 列名 1[, 列名 2, ...] from 表名 where 列 1 = 值 1 [and 列 2 = 值 2 and ...];
```

```text
select * from cpucode where     id =1 and addr = 'changsha';
```

![image-20200810114705475](https://gitee.com/cpu_code/picture_bed/raw/master//20200810114705.png)

```text
select addr from cpucode where id = 2 and name = 'cpu';
```

![image-20200810114823444](https://gitee.com/cpu_code/picture_bed/raw/master//20200810114823.png)

`or` 可在 where 子语句中把两个或多个条件结合起来（多个条件之间是或的关系） 。

匹配条件语法：

```text
where 列 1 = 值 1 [or 列 2 = 值 2 or ...]
```

```text
select * from 表名 where 列 1 = 值 1 [or 列 2 = 值 2 or ...];
```

```text
select 列名 1[,列名 2,...] from 表名 列 1 = 值 1 [or 列 2 = 值 2 or ...];
```

```text
select * from cpucode where id = 1 or addr = 'beijing';
```

![image-20200810115842372](https://gitee.com/cpu_code/picture_bed/raw/master//20200810115842.png)

```text
select name from cpucode where id = 3 or addr = 'shenzhen';
```

![image-20200810115958013](https://gitee.com/cpu_code/picture_bed/raw/master//20200810115958.png)

`between A and B` 会选取介于 A、 B 之间的数据范围。 这些值可以是数值、 文本或者日期。

匹配字符串时会以 ascii 顺序匹配。

不同的数据库对 between A and B 操作符的处理方式是有差异的。

* 有些数据库包含 A 不包含 B。  
* 有些包含 B 不包含 A  
* 有些既不包括 A 也不包括 B。  
* 有些既包括 A 又包括 B  

匹配条件语法：

```text
where 列名 between A and B
```

```text
select * from 表名 where 列名 between A and B;
```

```text
select 列名 1[,列名 2,...] from 表名 where 列名 between A and B;
```

```text
select * from cpucode where id between 1 and 3;
```

![image-20200810120415025](https://gitee.com/cpu_code/picture_bed/raw/master//20200810120415.png)

```text
select * from cpucode where addr between 'a' and 'f';
```

![image-20200810120425867](https://gitee.com/cpu_code/picture_bed/raw/master//20200810120426.png)

`like` 用于模糊查找

匹配条件语法：

若列值为数字 , 相当于列名＝列值

若列值为字符串 , 可以用通配符“ % ” 代表缺少的字符（一个或多个） 。

```text
where 列名 like 列值
```

```text
select * from cpucode where id like 2;
```

![image-20200810120638870](https://gitee.com/cpu_code/picture_bed/raw/master//20200810120639.png)

```text
select * from cpucode where name like '%u%';
```

![image-20200810120653093](https://gitee.com/cpu_code/picture_bed/raw/master//20200810120653.png)

`not` 可取出原结果集的补集

匹配条件语法：

```text
where 列名 not in 列值等
```

```text
where 列名 not in (列值 1, 列值 2, ...)
```

```text
where not (列 1 = 值 1 [and 列 2 = 值 2 and ...])
```

```text
where not (列 1 = 值 1 [or 列 2 = 值 2 or ...])
```

```text
 where 列名 not between A and B
```

```text
where 列名 not like 列值
```

```text
select * from cpucode where id not in (1);
```

![image-20200810123016883](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123017.png)

```text
select * from cpucode where addr not like '%zhen';
```

![image-20200810123116343](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123116.png)

order by 语句

根据指定的列对结果集进行排序。

默认按照升序对结果集进行排序， 可使用 `desc` 关键字按照降序对结果集进行排序。

升序

```text
select * from 表名 order by 列名;
```

降序

```text
select * from 表名 order by 列名 desc;
```

```text
select * from cpucode order by name;
```

![image-20200810123322828](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123323.png)

```text
select * from cpucode order by id;
```

![image-20200810123351083](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123351.png)

```text
select * from cpucode order by addr;
```

![image-20200810123412496](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123412.png)

```text
select * from cpucode order by id desc;
```

![image-20200810123337554](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123337.png)

### 事务

事务（Transaction） 可以使用 `BEGIN TRANSACTION` 命令或简单的 `BEGIN` 命令来启动。 此类事务通常会持续执行下去， 直到遇到下一个 `COMMIT` 或 `ROLLBACK` 命令。 不过在数据库关闭或发生错误时， 事务处理也会回滚。 以下是启动一个事务的简单语法：

在 SQLite 中， 默认情况下， 每条 SQL 语句自成事务。

`begin`： 开始一个事务， 之后的所有操作都可以取消

`commit`： 使 begin 后的所有命令得到确认。

`rollback`： 取消 begin 后的所有操作。

```text
begin;

delete from cpucode;

rollback;

select * from cpucode;
```

![image-20200810123809245](https://gitee.com/cpu_code/picture_bed/raw/master//20200810123809.png)

# SQL语句进阶

## 函数和聚合

函数：

SQL 语句支持利用函数来处理数据， 函数一般是在数据上执行的， 它给数据的转换和处理提供了方便常用的文本处理函数：

常用的文本处理函数：

```c
// 返回字符串的长度
length();
```

```c
//将字符串转换为小写
lower();
```

```c
// 将字符串转换为大写
upper();
```

语法：

```text
select 函数名(列名) from 表名;
```

```text
select * from cpucode;
```

![image-20200810143450457](https://gitee.com/cpu_code/picture_bed/raw/master//20200810143450.png)

```text
select id upper(name) from cpucode;
```

![image-20200810143440630](https://gitee.com/cpu_code/picture_bed/raw/master//20200810143440.png)

常用的聚集函数：

使用聚集函数， 用于检索数据， 以便分析和报表生成

```text
--返回某列的平均值
avg()
```

```text
--返回某列的行数
count()
```

```text
-- 返回某列的最大值
max()
```

```text
-- 返回某列的最小值
min()
```

```text
-- 返回某列值之和
sum()
```

插入一列分数 score

```text
alter table cpucode add score integer;
```

![image-20200810143923933](https://gitee.com/cpu_code/picture_bed/raw/master//20200810143924.png)

修改内容

```text
update cpucode set score=66 where name='cpu';
```

```text
update cpucode set score=77 where name='code';
```

```text
update cpucode set score=88 where name='test';
```

![image-20200810144150470](https://gitee.com/cpu_code/picture_bed/raw/master//20200810144150.png)

```text
select max(score) from cpucode;
```

![image-20200810144730589](https://gitee.com/cpu_code/picture_bed/raw/master//20200810144730.png)

```text
select avg(score) from cpucode;
```

![image-20200810144806248](https://gitee.com/cpu_code/picture_bed/raw/master//20200810144806.png)

```text
select count(*) from cpucode;
```

![image-20200810144817464](https://gitee.com/cpu_code/picture_bed/raw/master//20200810144817.png)

判断数据库中是否有 `cpucode` 这张表

`sqlite_master` 是数据库自带的一个表。 当用户创建一张表时， 数据库会将用户新建的表的信息存放在 `sqlite_master` 这张表中

```text
select count(*) from sqlite_master where type = 'table' and name = 'cpucode';
```

![image-20200810152755590](https://gitee.com/cpu_code/picture_bed/raw/master//20200810152755.png)

## 数据分组 group by

分组数据， 以便能汇总表内容的子集， 常和聚集函数搭配使用。

```text
select 列名 1[, 列名 2, ...] from 表名 group by 列名
```

```text
alter table cpucode add class text;

update cpucode set class='class_a' where name='cpu';
update cpucode set class='class_b' where name='code';
update cpucode set class='class_b' where name='test';
```

![image-20200810155059089](https://gitee.com/cpu_code/picture_bed/raw/master//20200810155059.png)

```text
select class, count(*) from cpucode group by class;
```

![image-20200810155155413](https://gitee.com/cpu_code/picture_bed/raw/master//20200810155155.png)

```text
select class, avg(score) from cpucode group by class;
```

![image-20200810155236737](https://gitee.com/cpu_code/picture_bed/raw/master//20200810155236.png)

group by 子句必须出现在 where 子句之后

```text
select class, avg(score) from cpucode where class='class_a' group by class;
```

![image-20200810155318529](https://gitee.com/cpu_code/picture_bed/raw/master//20200810155318.png)

## 过滤分组 having

除了能用 `group by` 分组数据外， 还可以包括哪些分组， 排除哪些分组。

通过 having 实现

语法：

```text
select 函数名（列名 1） [, 列名 2, ...] from 表名 group by 列名 having 函数名 限制值
```

```text
select class, avg(score) from cpucode group by class having avg(score) >=80;
```

![image-20200810155539900](https://gitee.com/cpu_code/picture_bed/raw/master//20200810155540.png)

## 约束

管理如何插入或处理数据库数据的规则

常用约束分类 :

**主键**、 **唯一约束**、 **检查约束**

主键：

惟一的标识一行\( 一张表中只能有一个主键 \)

主键应当是对用户没有意义的（ 常用于索引 ）

永远不要更新主键， 否则违反对用户没有意义原则

主键不应包含动态变化的数据， 如时间戳、 创建时间列、 修改时间列等

主键应当有计算机自动生成（ 保证唯一性 ）

语法：

```text
create table 表名称 (列名称1 数据类型 primary key, 列名称2 数据 类型,列名称3 数据类型, ...)；
```

唯一约束：

用来保证一个列（或一组列） 中数据唯一， 类似于主键， 但跟主键有区别

表可包含多个唯一约束， 但只允许一个主键

唯一约束列可修改或更新

创建表时， 通过 unique 来设置

语法:

```text
create table 表名 (列名称 1 数据类型 unique[， 列名称 2 数据类型 unique,...]);
```

```text
create table test (id integer primary key, name text unique);
```

![image-20200810160122651](https://gitee.com/cpu_code/picture_bed/raw/master//20200810160122.png)

```text
insert into test values(1, 'cpu');
```

```text
insert into test values(1, 'code');
```

![image-20200810164549086](https://gitee.com/cpu_code/picture_bed/raw/master//20200810164549.png)

```text
insert into test values(2, 'cpu');
```

![image-20200810164626151](https://gitee.com/cpu_code/picture_bed/raw/master//20200810164626.png)

检查约束：

用来保证一个列（或一组列） 中的数据满足一组指定的条件。

指定范围， 检查最大或最小范围， 通过 check 实现

```text
create table 表名 (列名 数据类型 check (判断语句));
```

```text
create table test2 (id integer, age integer check(age > 0));
```

![image-20200810164838299](https://gitee.com/cpu_code/picture_bed/raw/master//20200810164838.png)

```text
insert into test2 values(1, 30);
```

![image-20200810164927303](https://gitee.com/cpu_code/picture_bed/raw/master//20200810164927.png)

```text
insert into cpucode values(1, -20);
```

![image-20200810164941123](https://gitee.com/cpu_code/picture_bed/raw/master//20200810164941.png)

## 联结表（多表操作）

概念：

保存数据时往往不会将所有数据保存在一个表中， 而是在多个表中存储， 联结表就是从多个表中查询数据。

在一个表中不利于分解数据， 也容易使相同数据出现多次， 浪费存储空间； 使用联结表查看各个数据更直观，这使得在处理数据时更简单。

例如： 学生每年的考试成绩， 学生个人信息基本固定\(包括学号、 姓名、 地址等\)； 把所有信息放在同一个表中必然会造成学生的学号等基本信息重复。

对比：

学生信息和成绩在一个表中

单表缺点：

* 每年记录的成绩都需要添加重复的学生信息， 如： name， addr  
* lucy 的地址\(addr\)修改， 整个表所有的关于 lucy 的 addr 都需更改， 处理复杂。  

学生信息和成绩在不同的表中

学生信息\(persons\)：

学生成绩\(grade\)：

每个人的信息只需保存一份， 没有重复， 成绩 id 与学生信息 id 相同， 作为关联， 用于查找相应学生的成绩

lucy 的地址\(addr\)修改， 只需修改 persons 表中的 addr

分表优点：

将学生信息和成绩分开存储， 节省空间， 处理简单， 效率更高， 在处理大量数据时尤为明显。

使用关系型数据库存储数据， 各个表的设计是非常重要的， 良好的表设计， 能够简化数据的处理， 提高效率， 提高数据库的健壮性。

使用联结：

通过 select 语句将要联结的所有表以及它们如何关联

```text
select 列名 1,列名 2,.. from 表 1,表 2,.. where 判断语句;
```

```text
select name , addr, score, year from cpucode, grade where cpucode.id = grade.id;
```

在联结两个表时， 实际上是将第一个表中的每一行与第二个表中每一行配对， where 子句作为过滤条件，只有满足条件的才显示出来

匹配语句: persons.id = grade.id

完全限定列名， 用一个点\(.\)分隔表名和列名

终端输入\(输出指定学生的信息和分数\)：

```text
select name, addr, score, year from cpucode, grade where cpucode.id = greade.id and name='cpu';
```

select 语句中可以联结的表的数目没有限制

当前面指定列名二义性时， 需要通过完全限定名引用

## 视图（虚拟的表）

重用 SQL 语句

简化复杂的 SQL 操作\(如:多表查询\)

使用视图， 将整个查询包装成一个名为 PersonsGrade 的虚拟表， 简化了查询的 SQL 语句：

创建视图：

## 触发器

## 查询优化-索引

# SQLite的C编程

## 打开、 关闭数据库函数

sqlite3 使用了两个库： `pthread`、 `dl`， 故链接时应加上 `-lpthread` 和 `-ldl` 。

```c
/**
 * @function: 打开数据库
 * @parameter: 
 *        db_name：数据库文件名，UTF-8 编码
 *        db：数据库标识，此结构体为数据库操作句柄。 通过此句柄可对数据库文件进行相应操作。
 * @return {type} 
 *     success: SQLITE_OK
 *     error: 非 SQLITE_OK
 * @note: 
 */
int sqlite3_open(char *db_name,sqlite3 **db);
```

```c
/**
 * @function: 关闭数据库、 释放打开数据库时申请的资源。
 * @parameter: 
 *        db： 数据库的标识
 * @return {type} 
 *     success: SQLITE_OK
 *     error: 非 SQLITE_OK
 * @note: 
 */
int sqlite3_close(sqlite3 *db);
```

## Sqlite3 中执行 SQL 语句的方法 （回调方法）

```c
/**
 * @function: 执行 sql 指向的 SQL 语句， 若结果集不为空， 函数会调用函数指针 callback 所指向的函数。
 * @parameter: 
 *        db： 数据库的标识
 *        sql： SQL 语句（一条或多条），以’;’结尾
 *        callback： 是回调函数指针， 当这条语句执行之后， sqlite3 会去调用你提供的这个函数
 *        arg： 当执行 sqlite3_exec 的时候传递给回调函数的参数
 *        errmsg： 存放错误信息的地址， 执行失败后可以查阅这个指针
 * @return {type} 
 *     success: SQLITE_OK
 *     error: 非 SQLITE_OK
 * @note: 
 */
int sqlite3_exec(sqlite3 *db,
               const char *sql,
               exechandler_t callback,
               void *arg,
               char **errmsg);
```

```c
//打印错误信息方法
printf("%s\n", errmsg);
```

```c
/**
 * @function: 回调函数指针
 * @parameter: 
 *        para： sqlite3_exec 传给此函数的参数， para 为任意数据类型的地址。
 *        n_column： 结果集的列数。
 *        column_value： 指针数组的地址，其存放一行信息中各个列值的首地址
 *        column_name： 指针数组的地址，其存放一行信息中各个列值对应列名的首地址
 * @return {type} 
 *     success: 非 0 值， 则通知 sqlite3_exec 终止回调
 *     error: 
 * @note: 此函数由用户定义， 当 sqlite3_exec 函数执行 sql 语句后， 
 * 结果集不为空时 sqlite3_exec 函数会自动调用此函数， 每次调用此函数时会把结果集的一行信息传给此函数。
 */
typedef int (*exechandler_t)(void *para,
                       int n_column,
                       char **column_value,
                       char **column_name);
```

栗子 sqlite3\_exec .c :

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sqlite3.h>

int main(int argc,char **argv)
{
    sqlite3 *db = NULL;
    char *sql = NULL;
    int result = 0;
    char *errmsg;

    result = sqlite3_open("test.db",&db);

    if(result != SQLITE_OK)
    {
        printf("open error\n");
        return -1;
    }

    sql = "insert into cpucode values (4,'test','beijing');";

    sqlite3_exec(db,sql ,NULL, NULL, &errmsg);
    if(errmsg)
    {
        printf("errmsg = %s\n", errmsg);
    } 

    sqlite3_close(db);
    return 0;
}
```

Makefile

```text
OBJ += sqlite3_exec.o
OBJ += sqlite3.o

FLAGS = -Wall
CC = gcc

example:$(OBJ)  
    $(CC) $(OBJ) -o $@ $(FLAGS) -lpthread -ldl
%.o:%.c
    $(CC) -c $^ -o $@ $(FLAGS)

.PHONY:clean
clean:
    rm example *.o -rfv
```

## Sqlite3 中执行 SQL 语句的方法 （非回调方法）

```c
/**
 * @function: 关闭数据库、 释放打开数据库时申请的资源。
 * @parameter: 
 *        db： 数据库的标识
 * @return {type} 
 *     success: SQLITE_OK
 *     error: 非 SQLITE_OK
 * @note: 
 */
int sqlite3_get_table(sqlite3 *db,
               const char *sql,
               char ***resultp,
               int *nrow,
               int *ncolumn,
               char **errmsg);
```

```c
/**
 * @function: 释放 sqlite3_get_table 分配的内存
 * @parameter: 
 *        resultp： 结果集数据的首地址。
 * @return {type} 
 *     success: 
 *     error: 
 * @note: 
 */
void sqlite3_free_table(char **resultp);
```

