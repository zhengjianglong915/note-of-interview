# SQL语句汇总
## 一. 数据库相关
#### 1) 连接数据库

格式：mysql -h主机地址 -u用户名 －p用户密码

例如：

```
mysql -h localhost -uroot -p 
```

#### 2) 创建数据库

命令：create database <数据库名>
例1：建立一个名为test_db的数据库

```
CREATE DATABASE test_db
```

#### 3) 删除数据库

```
DROP DATABASE dbname
```

#### 4) 增加用户
格式：grant select on 数据库.* to 用户名@登录主机 identified by “密码”

- 增加一个用户test1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限

    ```
    grant select,insert,update,delete on *.* to [email=test1@"%]test1@"%[/email]" Identified by "abc";
    ```
    
    但增加的用户是十分危险的，你想如某个人知道test1的密码，那么他就可以在internet上的任何一台电脑上登录你的mysql数据库并对你的数据可以为所欲为了，解决办法见2.2。


- 增加一个用户test2密码为abc，让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作
    
    这样用户即使用知道test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问。
    
    ```
    rant select,insert,update,delete on mydb.* to [email=test2@localhost]test2@localhost[/email] identified by "abc";
    ```

- 如果你不想test2有密码，可以再打一个命令将密码消掉。

    ```
    grant select,insert,update,delete on mydb.* to [email=test2@localhost]test2@localhost[/email] identified by ""
    ```

#### 5）显示数据库
命令：show databases （注意：最后有个s）

```
mysql> show databases
```

#### 6) 使用数据库
命令： use <数据库名>
例如：如果test_db数据库存在，尝试存取它：

```
mysql> use test_db;
```

use 语句可以通告MySQL把db_name数据库作为默认（当前）数据库使用，用于后续语句。该数据库保持为默认数据库，直到语段的结尾，或者直到发布一个不同的USE语句：

```
mysql> USE db1;
mysql> SELECT COUNT(*) FROM mytable; # selects from db1.mytable
mysql> USE db2;
mysql> SELECT COUNT(*) FROM mytable; # selects from db2.mytable
```

#### 7) 当前选择数据库
MySQL中SELECT命令类似于其他编程语言里的print或者write，你可以用它来显示一个字符串、数字、数学表达式的结果等等。

- **显示MYSQL的版本**: select version(); 
- **显示当前时间**: select now();
- **显示年月日**: SELECT DAYOFMONTH(CURRENT_DATE);
- **显示字符串**: SELECT "welecome to my blog!"; 
- **当计算器用**: select ((4 * 4) / 10 ) + 25; 
- **串接字符串**: select CONCAT(f_name, " ", l_name) AS Name from employee_data; 这里用到CONCAT()函数，用来把字符串串接起来。

## 二. 表相关
### 2.1 创建新表
#### 1) 创建新表

命令：

```
create table tabname(col1 type1 [not null] [primary key],col2 type2 [not null],..)
```

比较简单的使用方式：

```
create table MyClass(
  id int(4) not null primary key auto_increment,
  name char(20) not null,
  sex int(4) not null default '0',
   degree double(16,2));
```

较为完整的方式：

```
CREATE TABLE `t_books` (
  `F_id` varchar(32) NOT NULL COMMENT '主键',
  `F_book_id` int(10) unsigned NOT NULL COMMENT '书本号',
  `F_publish_id` int(20) unsigned NOT NULL COMMENT '厂商ID',
  `F_type` tinyint(2) unsigned NOT NULL DEFAULT '0' COMMENT '书本类型',
  `F_sell_amount` bigint(20) NOT NULL DEFAULT '0' COMMENT '金额(保留到分)',
  `F_desc` varchar(256) DEFAULT NULL DEFAULT '' COMMENT '描述',
  `F_create_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
  `F_modify_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '最近更新时间',
  PRIMARY KEY (`F_id`),
  KEY `idx_book_id` (`F_book_id`) COMMENT '索引：书本',
  KEY `idx_book` (`F_type`, `F_publish_id`) COMMENT '索引'
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='书本';
```

根据已有的表创建新表：

```
A：create table tab_new like tab_old (使用旧表创建新表)
B：create table tab_new as select col1,col2… from tab_old definition only
```

#### 2）获取表结构
命令： desc 表名，或者show columns from 表名

```
mysql> desc t_books;
mysql> show columns from t_books;
```

使用MySQL数据库desc 表名时，我们看到Key那一栏，可能会有4种值，即' '，'PRI'，'UNI'，'MUL'。

- **如果Key是空的**, 那么该列值的可以重复, 表示该列没有索引, 或者是一个非唯一的复合索引的非前导列；
- **如果Key是PRI**,  那么该列是**主键**的组成部分；
- **如果Key是UNI**,  那么该列是一个**唯一值索引**的第一列(前导列),并别**不能含有空值(NULL)**；
- **如果Key是MUL**,  那么该列的值**可以重复**, 该列是一个非唯一索引的前导列(第一列)或者是一个

唯一性索引的组成部分但是可以含有空值NULL。如果对于一个列的定义，同时满足上述4种情况的多种，比如一个列既是PRI,又是UNI，那么"desc 表名"的时候，显示的Key值按照优先级来显，PRI->UNI->MUL。那么此时，显示PRI。

- 一个唯一性索引列可以显示为PRI,并且该列不能含有空值，同时该表没有主键。
- 一个唯一性索引列可以显示为MUL, 如果多列构成了一个唯一性复合索引，因为虽然索引的多列组合是唯一的，比如ID+NAME是唯一的，但是没一个单独的列依然可以有重复的值，只要ID+NAME是唯一的即可。

#### 3) 删除新表

```
drop table tabname
```

#### 4) 向表插入数据
命令：insert into <表名> [( <字段名1>[,..<字段名n > ])] values ( 值1 )[, ( 值n )]


往表 MyClass中插入二条记录, 这二条记录表示：编号为1的名为Tom的成绩为96.45, 编号为2 的名为Joan 的成绩为82.99， 编号为3 的名为

Wang 的成绩为96.5。

```
mysql> insert into MyClass values(1,'Tom',96.45), (2,'Joan',82.99), (2,'Wang', 96.59);
```

注意：insert into每次只能向表中插入一条记录。


### 2.2 修改表
#### 1) 增加一个列

```
Alter table tabname add column col type
```

注：列增加后将不能删除。DB2中列加上后数据类型也不能改变，唯一能改变的是增加varchar类型的长度。

#### 2）添加主键

```
Alter table tabname add primary key(col)
```

#### 3）删除主键

```
Alter table tabname drop primary key(col)
```

#### 4）创建索引

```
create [unique] index idxname on tabname(col….)
```

#### 5) 删除索引

```
drop index idxname
```

索引是不可更改的，想更改必须删除重新建。

#### 6) 修改表名

命令：rename table 原表名 to 新表名;
例如：在表MyClass名字更改为YouClass

```
mysql> rename table MyClass to YouClass;
```

#### 7) 创建视图

## 复杂命令
#### UNION
UNION 运算符通过组合其他两个结果表（例如 TABLE1 和 TABLE2）并消去表中任何重复行而派生出一个结果表。当 ALL 随 UNION 一起使用时（即 UNION ALL），不消除重复行。两种情况下，派生表的每一行不是来自 TABLE1 就是来自 TABLE2。

#### EXCEPT 运算符
EXCEPT 运算符通过包括所有在 TABLE1 中但不在 TABLE2 中的行并消除所有重复行而派生出一个结果表。当 ALL 随 EXCEPT 一起使用时 (EXCEPT ALL)，不消除重复行。


#### INTERSECT 运算符
INTERSECT 运算符通过只包括 TABLE1 和 TABLE2 中都有的行并消除所有重复行而派生出一个结果表。当 ALL 随 INTERSECT 一起使用时 (INTERSECT ALL)，不消除重复行。
注：使用运算词的几个查询结果行必须是一致的。

#### 使用外连接


## 简单sql
- **选择**：select * from table1 where 范围
- **插入**：insert into table1(field1,field2) values(value1,value2)
- **删除**：delete from table1 where 范围
- **更新**：update table1 set field1=value1 where 范围
- **查找**：select * from table1 where field1 like ’%value1%’ ---like的语法很精妙，查资料!
- **排序**：select * from table1 order by field1,field2 [desc]
- **总数**：select count as totalcount from table1
- **求和**：select sum(field1) as sumvalue from table1
- **平均**：select avg(field1) as avgvalue from table1
- **最大**：select max(field1) as maxvalue from table1
- **最小**：select min(field1) as minvalue from table1

## 其他
### 备份数据

命令在mysql安装目录的bin文件夹下执行。

#### 1) 导出整个数据库
导出文件默认是存在mysql\bin目录下
mysqldump -u 用户名 -p 数据库名 > 导出的文件名

```
mysqldump -u user_name -p123456 database_name > outfile_name.sql
```

#### 2) 导出一个表

命令： mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名

```
mysqldump -u user_name -p database_name table_name > outfile_name.sql
```

#### 3) 导出一个数据库结构

命令： mysqldump -u 用户名 -p -d –add-drop-table 数据库名 > 导出的文件名

```
mysqldump -u user_name -p -d –add-drop-table database_name > outfile_name.sql
```

-d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table

#### 4）带语言参数导出

```
mysqldump -uroot -p –default-character-set=latin1 –set-charset=gbk –skip-opt database_name > outfile_name.sql
```

例如，将test_db库备份到文件back_test_db中：

代码如下:

```
mysqldump -u root -p --opt test_db > back_test_db
```

## 提升
#### 复制表
只复制结构,源表名：a 新表名：b 

```
select * into b from a where 1<>1（仅用于SQlServer） // 
select top 0 * into b from a;  // 状态
```

#### 拷贝表
拷贝数据,源表名：a 目标表名：b

```
insert into b(a, b, c) select d,e,f from b;
```

#### 跨数据库之间表的拷贝
具体数据使用绝对路径

```
insert into b(a, b, c) select d,e,f from b in ‘具体数据库’ where 条件
```

#### 拷贝表

```
insert into b(a, b, c) select d,e,f from b;
```

#### 子查询

```
select a,b,c from a where a IN (select d from b ) 
```

或者:

```
select a,b,c from a where a IN (1,2,3)
```

#### 两张关联表，删除主表中已经在副表中没有的信息

```
delete from table1 where not exists ( select * from table2 where table1.field1=table2.field1)
```

#### 随机取出10条数据

```
select top 10 * from tablename order by newid()
```

#### 随机选择记录

```
select newid()
```

#### 执行外部sql

- 启动时执行

```
mysql -uroot -p密码 < ~/school.sql
```

- 在mysql命令行中执行

```
mysql> source ~/school.sql;
```


https://www.cnblogs.com/jaechen/p/8028162.html
http://www.jb51.net/article/74564.htm


https://www.cnblogs.com/jaechen/p/8028162.html


