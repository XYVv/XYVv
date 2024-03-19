+++
author = "xinyu"
title = "开发工具——sqlite"
date = "2024-03-19"
description = "开发工具专题之sqlite"
categories = [
    "算法","排序"
]
tags = [
    "开发工具","sqlite"
]
image = "1.jpg"

+++

![](2.jpg)

> sqlite数据库 / mysql数据库
> sqlite属于轻量级数据库，广泛应用与嵌入式产品中，本项目中暂且选择sqlite数据库
>
> ubuntu环境安装
>
> 1. ```bash
>    sudo apt install sqlite3 libsqlite3-dev
>    ```
>
> 2. 数据库数据类型
>    int real char text NULL
>
> 3. 基本操作
>
> 创建数据表格
> create  table  表名( 字段名 数据类型  ， 字段名 数据类型， 字段名  数据类型， 字段名  数据类型)；
> create table student(number text(256), name text(256), address text);
>
> 插入表格
> insert into 表名  values('字段数据'，'字段数据'，'字段数据'，'字段数据' );
> insert into student values('20230101', '张三', '北京');
> insert into student values('20230102', '李四', '上海');
>
> 查询数据
> select  字段名...字段名  from  表名；
> select * from student ;
> select name, number from student;
> select  字段名...字段名  from  表名  where 条件;
>
> 更新数据
> update 表名 set 字段1=字段1值, 字段2=字段2值… where 条件表达式;
> update student set number='199999999999' where name='岳飞';
>
> 删除数据
> delete  from 表名;//删除整个表数据，不会删除表格
> delete  from 表名  where  条件；
> delete from student where number='20230103';
>
> 查询创建表命令
> .schema 表名

# C接口

## (1)打开一个sqlite3的数据库连接对象 

```c
SQLITE_API int sqlite3_open
(
  const char *filename,   //你要打开的数据库的路径名
  sqlite3 **ppDb         //二级指针 用来保存打开的数据库连接对象
);
		
sqlite3* 用来表示你打开的那个sqlite3的数据库文件（test.db） 后续对数据库的所有操作都是通过该文件。 

返回值： 
		成功返回SQLITE_OK 
		失败返回其他值
```

## (2)关闭数据库连接对象  

```c
SQLITE_API int sqlite3_close(sqlite3*); 
参数表示你要关闭哪个打开的数据库连接对象 
```

## (3)准备好一个SQL语句对象 

```c
使用sqlite3_stmt这个结构来描述一个准备好的SQL语句对象
我们的应用都是通过SQL语句对象去发送SQL指令 

	SQLITE_API int sqlite3_prepare_v2
	(
	  sqlite3 *db,            /* Database handle */
	  const char *zSql,       /* SQL statement, UTF-8 encoded */
	  int nByte,              /* Maximum length of zSql in bytes. */
	  sqlite3_stmt **ppStmt,  /* OUT: Statement handle */
	  const char **pzTail     /* OUT: Pointer to unused portion of zSql */
	);
				
	函数功能： 把一个字符串类型的SQL语句命令转换成可以使用的语句对象类型sqlite3_stmt
	
	参数列表： 
			db: 数据库连接对象  表示你得语句将要作用在哪一个数据库上 
			
			zSql: 你要执行的SQL语句的字符串格式  
				
			nByte：SQL语句的长度 或者是 你要编译到zSql字符串的哪一个位置 		
				   原始的数据库字符串zSql有可能包含多条SQL语句
				   
					>0  编译到zSql指向的sql语句的前nBytes个字节 
					<0  编译到zSql指向的sql语句的第一个'\0'为止 
					=0  什么都不编译

			ppStmt：二级指针 用来保存编译好后的SQL语句对象  		

			pzTail ：指向原始SQL语句字符串zSql中未使用的部分 一般给0 或者NULL

返回值： 
			成功返回SQLITE_OK 
			失败返回其他值
```

## (4) 执行准备好的SQL语句对象 

```c
SQLITE_API int sqlite3_step(sqlite3_stmt*);	

	参数:
		就是你要执行的SQL语句对象
	
	返回值：
		成功返回SOLITE_DONE 		
		失败返回其他值
```

## (5) 释放语句对象资源  

```c
SQLITE_API int sqlite3_finalize(sqlite3_stmt *pStmt);
```

## (6) sqlite3_exec内部封装了三个函数[sqlite3_prepare_v2 sqlite3_step sqlite3_finalize]

他是一个万能函数 它可以让应用程序执行多个SQL语句 而不需要使用sqlite3中其他的大量的接口函数 

```c
SQLITE_API int sqlite3_exec
(
  sqlite3*,                                  /* An open database */
  const char *sql,                           /* SQL to be evaluated */
  int (*callback)(void*,int,char**,char**),  /* Callback function */
  void *,                                    /* 1st argument to callback */
  char **errmsg                              /* Error msg written here */
);

函数功能： 指定一条指定的sql语句 

参数列表： 
         第一个参数： 一个打开的数据库连接对象
         
         第二个参数： 是你要准备执行的sql语句 
         
         第三个参数： 回调函数： 当执行sqlite3_exec后 有可能会执行此回调函数 
					  填NULL 就表示你不需要执行回调函数
					  什么时候需要回调函数？ 当你做SELECT时 就需要回调函数。 
					  那么SELECT时候的回调函数是干嘛？ 就是把你select查询的结果输出出来 
					  select产生多少条结果 回调函数就会执行多少次。
					  
         第四个参数： 你需要传递到回调函数中的信息 类似于pthread-create中给线程函数传参 
				  如果你不需要传递任何消息 就填NULL 如果多个消息 请自行封装成结构体
				  
		第五个参数： 二级指针 保存一级指针的地址  保存执行的出错的信息  
				  一旦出错 则将错误信息写入[sqlite3_malloc]分配的空间中 并且让第5个参数保存分配的空间的地址 
				  为了避免内存泄露 应用程序在处理完错误信息之后 需要调用sqlite3_free释放掉第五个参数指向的内容空间
```

