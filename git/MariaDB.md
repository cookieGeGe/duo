### 关系型数据库
**关系数据库**（英语：Relational database），是创建在[关系模型](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E6%A8%A1%E5%9E%8B "关系模型")基础上的[数据库](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93 "数据库")，借助于[集合代数](https://zh.wikipedia.org/wiki/%E9%9B%86%E5%90%88%E4%BB%A3%E6%95%B0 "集合代数")等[数学](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6 "数学")概念和方法来处理数据库中的数据。现实世界中的各种[实体](https://zh.wikipedia.org/wiki/%E5%AF%A6%E9%AB%94 "实体")以及实体之间的各种联系均用[关系模型](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E6%A8%A1%E5%9E%8B "关系模型")来表示。关系模型是由[埃德加·科德](https://zh.wikipedia.org/wiki/%E5%9F%83%E5%BE%B7%E5%8A%A0%C2%B7%E7%A7%91%E5%BE%B7 "埃德加·科德")于1970年首先提出的，并配合“[科德十二定律](https://zh.wikipedia.org/wiki/%E7%A7%91%E5%BE%B7%E5%8D%81%E4%BA%8C%E5%AE%9A%E5%BE%8B "科德十二定律")”。现如今虽然对此模型有一些批评意见，但它还是数据存储的传统标准。标准数据查询语言[SQL](https://zh.wikipedia.org/wiki/SQL "SQL")就是一种基于关系数据库的语言(SQL不是基于关系数据库的语言)，这种语言执行对关系数据库中数据的检索和操作。

[关系模型](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E6%A8%A1%E5%9E%8B "关系模型")由关系数据结构、关系操作集合、关系完整性约束三部分组成。------引自维基百科

### MariaDB
**MariaDB**数据库管理系统是[MySQL](https://zh.wikipedia.org/wiki/MySQL "MySQL")的一个分支，主要由开源社区在维护，采用[GPL](https://zh.wikipedia.org/wiki/GPL "GPL")授权许可。开发这个分支的原因之一是：[甲骨文公司](https://zh.wikipedia.org/wiki/%E7%94%B2%E9%AA%A8%E6%96%87%E5%85%AC%E5%8F%B8 "甲骨文公司")收购了MySQL后，有将MySQL[闭源](https://zh.wikipedia.org/wiki/%E9%97%AD%E6%BA%90 "闭源")的潜在风险，因此社区采用分支的方式来避开这个风险。<sup>[[4]](https://zh.wikipedia.org/wiki/MariaDB#cite_note-4)</sup>

MariaDB的目的是完全兼容MySQL，包括[API](https://zh.wikipedia.org/wiki/API "API")和命令行，使之能轻松成为MySQL的代替品。在存储引擎方面，10.0.9版起使用[XtraDB](https://zh.wikipedia.org/w/index.php?title=XtraDB&action=edit&redlink=1 "XtraDB（页面不存在）")（名称代号为[Aria](https://zh.wikipedia.org/w/index.php?title=Aria_(%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)&action=edit&redlink=1)）来代替MySQL的[InnoDB](https://zh.wikipedia.org/wiki/InnoDB "InnoDB")。------引自维基百科

### MariaDB和MySql的区别
其实MariaDB和MySql是没有什么区别的，在Sun公司被Oracle收购后，按照Oracle的风格MySql是肯定会被闭源的，考虑到这种情况下所以MySql之父就另开了MariaDB分支。
MariaDB跟MySQL在绝大多数方面是兼容的，对于开发者来说，几乎感觉不到任何不同。目前MariaDB是发展最快的MySQL分支版本，新版本发布速度已经超过了Oracle官方的MySQL版本。
MariaDB 是一个采用Aria存储引擎的MySQL分支版本，是由原来 MySQL 的作者Michael Widenius创办的公司所开发的免费开源的数据库服务器。

### SQL
SQL是一个标准的数据库语言，是面向集合的描述性非过程化语言。
它功能强，效率高，简单易学易维护。SQL语言分为以下四大类：
DDL、DML、DCL、DQL。

### DDL（数据库定义语言）
用来创建数据库中的各种对象-----表、视图、索引、同义词、聚簇等，常用的命令如create, drop, alter等

- create：用来创建数据库，表，索引，视图等。常用语法：
```
# 创建数据库：
CREATE DATABASE [IF NOT EXISTS] db_name；

# 创建表：
CREATE TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)
    [table_options]；
```
- drop:用来删除数据库，表，索引，视图等。常用语法：
```
# 删除数据库
DROP DATABASE [IF EXISTS] db_name；

# 删除表
DROP TABLE [IF EXISTS]  tbl_name [, tbl_name] ..；
```
- alter：用来修改数据库，表或者表字段，索引，视图等信息。常用语法：
```
# 修改数据库信息
ALTER DATABASE [db_name] [DEFAULT] CHARACTER SET [=] charset_name；

# 修改表的信息：
ALTER TABLE tbl_name [alter_specification [, alter_specification] ...];

#alter_specification可以是一下内容：
alter_specification：
    ADD [COLUMN] col_name column_definition
        [FIRST | AFTER col_name ]
|   ADD [CONSTRAINT] PRIMARY KEY
|   ALTER [COLUMN] col_name {SET DEFAULT value | DROP DEFAULT}
|   DROP [COLUMN] col_name
|   DROP PRIMARY KEY
|   CHANGE [COLUMN] old_col_name new_col_name column_definition
        [FIRST|AFTER col_name]
|   MODIFY [COLUMN] col_name column_definition
        [FIRST | AFTER col_name];
```
#### DML（数据库操纵语言）
DML是对数据中的表数据的可以执行的操作的语言。数据操作指令包括：update、insert、delete。
- insert：向指定的数据表中插入数据，可以是一条或者多条数据，语法：
```
INSERT [INTO] tbl_name [(col_name,...)]
    {VALUES | VALUE} (value1),(value2)...;
```
- update：用来修改表中的数据。语法：
```
UPDATE  table_name
    SET col_name1={expr1|DEFAULT} [, col_name2={expr2|DEFAULT}] ...
    [WHERE where_condition];
```
- delete：删除表中的数据。语法：
```
DELETE [col_name]  FROM tbl_name
    [WHERE where_condition];
```
>**注意：**在更新和删除表里面的数据时一定要使用where字句，否则默认是操作所有行，在delete中语句中如果指定col_name则是删除该字段的数据。

### DCL（数据库控制语言）
DCL用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。对事务的处理主要是rollback和commit。主要还是对权限的控制。语法如下：
```
GRANT privileges ON database.table TO 'username'@'host' IDENTIFIED BY('password');
```
>说明：database,table用*表示所有,host用%表示任意参数。
### DQL（数据库查询语言）
数据查询语言DQL基本结构是由SELECT子句，FROM子句，WHERE
子句组成。语法：
```
SELECT
    [ALL | DISTINCT  ]  select_expr1 [, select_expr2 ...]
    [FROM table_references
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC] [HAVING where_condition]]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
```
这里只是初略的介绍一下sql语句的一些基础用法，其实在sql中其实最重要的是DQL语句中的一些高级应用。如子查询，复合查询等。这将在后边的文章里面介绍。

#### 练习：

学生选课系统
1. 创建学生选课系统
2. 切换数据库
3. 创建学生表 TbStudent，主键stuid ，姓名stuname，性别stusex，生日stubirth，电话stutel，住址stuaddr，照片stuphoto（以二进制存）
4. 创建课程表TbCourse
主键cosid， 班级名称cosname，学分coscredit，课程描述cosintro
5. 学生选课记录表TbSC
主键scid，学生外键sid ，班级外键cid，创建日期scdate， 分数score

创建语句：
```
# 创建SCC数据库，默认字符集为utf-8：
create database if not exists SCC default charset utf8;

use SCC;

#创建tbstudent学生表，默认字符集为utf-8，存储引擎为innodb：
create table if not exists tbstudent(
stuid int(15) not null primary key,
stuname varchar(20) not null,
stusex tinyint(1) default 1,
stubirth datetime,
stutel varchar(11),
stuaddr varchar(255),
stuphoto longblob
)engine innodb default charset utf8;

# 创建tbcourse课程表，默认字符集为utf-8，存储引擎为innodb：
create table if not exists tbcourse(
cosid int not null primary key,
cosname varchar(20) not null,
coscredit int not null,
cosintro varchar(200)
)engine innodb default charset utf8;

#创建tbsc选课记录表，默认字符集为utf-8，存储引擎为innodb：
create table if not exists tbsc(
scid int not null primary key auto_increment,
sid int not null,
cid int not null,
scdate datetime not null,
score decimal(3,1)
)engine innodb default charset utf8;

# 为三张表创建外键约束：
aler table tbsc add constrait fs_sid foreign key(sid) references tbstudent(stuid) on delete cascade on update cascade;
aler table tbsc add constrait fs_cid foreign key(cid) references tbsourse(cosid) on delete set null on update cascade

# 向学生表中插入数据：
insert into tbstudent (stuid, stuname, stusex, stubirth, stuaddr, stuphoto) values
(1001, '张三丰', default, '1978-1-1', '成都市一环路西二段17号', null);
insert into tbstudent (stuid, stuname,stubirth) values
(1002, '郭靖', '1980-2-2');
insert into tbstudent (stuid, stuname, stusex, stubirth, stuaddr) values (1003, '黄蓉', 0, '1982-3-3', '成都市二环路南四段123号');
insert into tbstudent (stuid, stuname, stusex, stubirth, stuaddr, stuphoto) values 
(1004, '张无忌', 1, '990-4-4', null, null),
(1005, '丘处机', 1, '1983-5-5', '北京市还定去宝胜北里西区28号', null),
(1006, '王处一', 1, '1985-6-6', '深圳市宝安区宝安大道5010号',null),
(1007, '刘处玄', 1, '1987-7-7', '郑州市金水区纬五路21号', null),
(1008, '孙不二', 0, '1989-8-8', '武汉市光谷大道61号', null),
(1009, '平一指', 1, '1992-9-9', '西安市雁塔区高新六路52号', null),
(1010, '老不死', 1, '1993-10-10', '广州市天河区元岗路310号', null),
(1011, '王大锤', 0, '1994-11-11', null, null),
(1012, '隔壁老王', 1, '1995-12-12', null, null),
(1013, '郭啸天', 1, '1977-10-25', null, null);

# 删除学生表中id为1004的学生信息：
delete from tbstudent where stuid=1004;

# 更新学生表中id为1002的学生的信息：
update tbstudent set stubirth='1980-12-12',stuaddr='上海市宝山区同济支路199号' where stuid=1002;

# 向课程标准插入数据
insert into tbcourse values 
(1111, 'C语言程序设计', 3,'大神级讲师教授需要抢座'),
(2222, 'Java程序设计', 3, null),
(3333, '数据库概论', 2, null),
(4444, '操作系统原理', 4, null);

# 向学生选课记录表中插入数据：
insert into tbsc (sid, cid, scdate, score) values
(1001, 1111, '2016-9-1', 95),
(1002, 1111, '2016-9-1', 94),
(1001, 2222, now(), null),
(1001, 3333, '2017-3-1', 85),
(1001, 4444, now(), null),
(1002, 4444, now(), null),
(1003, 2222, now(), null),
(1003, 3333, now(), null),
(1005, 2222, now(), null),
(1006, 1111, now(), null),
(1006, 2222, '2017-3-1', 80),
(1006, 3333, now(), null),
(1006, 4444, now(), null),
(1007, 1111, '2016-9-1', null),
(1007, 3333, now(), null),
(1007, 4444, now(), null),
(1008, 2222, now(), null),
(1010, 1111, now(), null);
```


