### 聚合函数
聚合函数对一组值执行计算，并返回单个值。 除了 COUNT 以外，聚合函数都会忽略空值。 聚合函数经常与 SELECT 语句的 GROUP BY 子句一起使用。常用的聚合函数有：

- sum(expr):求和
- avg(expr):求平均数
- count(expr):计数器，返回SELECT语句检索到的行中非NULL值的数目
- max(expr) 获取最大值
- min(expr) 获取最小值

### mysql查询子句

#### where（条件查询）

在where条件查询中可以使用比较运算符，逻辑运算符，in,between  and和模糊查询：
- 比较运算符包括：>, =, <, >=, <=, !=
- 逻辑运算符：and, or, not
- in：类似python中的in
- between and：表示在什么之间
- 模糊查询like：类似正则表达式的用法，用通配符去匹配字符，但是在这里的通配符只有两个，%匹配任意长度的任意字符，_匹配任意单个字符。

例如：

1.学生成绩为60分到90分之间的有哪些？
```where scores between 60 and 90```

2.查找学生姓王的学生？
```where name like '王%'```

### group by（分组）
将查询的结果按照某个字段的值进行分组，如果需要对分组后的数据进行筛选就还需要用到having子句，而不是where子句。

例如：
查找数学英语这两门课的平均成绩大于70分的学生：（假设表scores中有数学英语成绩和姓名）
```
select name,avg(score) as c from scores group by(name) having c >70;
```
### as（别名）

对于一些名字比较长的字段或者表名，我们可以给它一个比较简短且全局唯一的别名，然后通过别名去引用它（查询语句也可以做别名）。

例如：
```
select name,age from student as st;
select avg(age) as avg_age from student;
```

### order by（排序）
通常在处理数据或者查询数据的时候往往都需要进行排序，sql中使用order by对相应的数据进行排序，默认查询结果是按照(asc)升序排列的，要降序排列使用desc。

例如：
查询学生的成绩，并按照降序排列
```select name,score from scores order by score desc;```

### if（判断）

if(字段,exp1,exp2) 或者 ifnull(字段,exp1,,exp2) 作用：if表达式中如果字段值为真则返回exp1的值，如果为假的话，返回exp2的值     ifnull表达式中如果字段的值为假则返回exp1的值，如果为假的话，返回exp2的值。

例如： 查询男女学生的人数(分组和聚合函数)
```
select if(stusex, '男', '女') as '性别', count(stusex) as '人数' from TbStudent group by stusex;
```
### 去重和显示结果条数
使用distinct(字段名)表示去除字段名中重复的项。
limit子句用来现在结果的显示条数，用法为limit [offset],N其中offset可以不设置默认为0，如果设置则表示偏移offset条信息然后显示N条信息。offset也可以单独使用。

例如:
取出成绩表中在第四个到第六个这三个人：
```
select name from scores limit 3,3;
```
### 子查询
- where型子查询
把内层查询结果当作外层查询的比较条件

例如：
不适用排序，找出最高分的学生姓名和成绩
```select name,score from scores where score = (select max(score) from scores);```

- from型子查询
把内层的查询结果供外层再次查询

例如：
不用group by，查询平均成绩大于等于90分的学生的学号和平均成绩：
```select ts1.stuid,t2.avg from tbstudent ts1,(select sid,avg(score) as avg from tbsc group by(sid)) as t2 where t2.sid=ts1.stuid and t2.avg>=90;```

- exists型子查询
把外层查询结果拿到内层，看内层的查询是否成立

例如：查询哪些栏目下有商品，栏目表category,商品表goods
```select cat_id,cat_name from category where exists(select * from goods where goods.cat_id = category.cat_id);```

### 联接

定义 A INNER/LEFT/RIGHT JOIN B操作中，A表被称为左表，B表被称为右表。
a) 内关联： Inner Join on 作用：仅对满足连接条件的列进行关联，其中inner可省略

b) 左外连接：Left Outer Jion on 作用：其中outer可以省略。如A LEFT JOIN B，会输出左表A中所有的数据，同时将符合ON条件的右表B中搜索出来的结果合并到左表A表中，如果A表中存在而在B表中不存在，则结果集中会将查询的B表字段值（如此处的P.PUNISHMENT字段）设置为NULL。 所以，LEFT JOIN的作用是： LEFT JOIN：从右表B中将符合ON条件的结果查询出来，合并到A表中，再作为一个结果集输出。

c) 右外连接：Right Outer Jion on 作用：其中outer可以省略，而RIGHT JOIN刚好相反，“A RIGHT JOIN B ON ……”是将符合ON条件的A表搜索结果合并到B表中，作为一个结果集输出：

### 练习：

以前一篇[MariaDB基础](https://www.jianshu.com/p/1078c79bc558)中的练习题创建的数据库为基础

![tbcourse表](https://upload-images.jianshu.io/upload_images/10930505-1ec8170418a6e22a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![tbsc表](https://upload-images.jianshu.io/upload_images/10930505-5ab6a2577937a63a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![tbstudent表](https://upload-images.jianshu.io/upload_images/10930505-bb55aaf05523382f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

习题如下：

1.查询所有学生信息：
```select * from tbstudent;```

2.查询所有课程名称及学分
```select cosname,sum(coscredit) from tbcourse join tbsc on tbcourse.cosid=tbsc.cid group by(tbcourse.cosname);```

3.查询所有女学生的姓名和出生日期（筛选）
```select stuname,stubirth from tbstudent where stusex=0;```

4.查询所有80后学生的姓名、性别和出生日期（筛选）
```select stuname,stusex,stubirth from tbstudent where stubirth between '1980-1-1' and '1990-1-1';```

5.查询姓王的学生姓名和性别（模糊）
```select stuname,stusex from tbstudent where stuname like '王%';```

6.查询姓郭名字总共两个字的学生的姓名（模糊）
```select stuname from tbstudent where stuname like '郭_';```

7.查询姓郭名字总共三个字的学生的姓名（模糊）
```select stuname from tbstudent where stuname like '郭_';```

8.查询名字中有王字的的学生的姓名（模糊）
```select stuname from tbstudent where stuname like '%王%';```

9.查询没有录入家庭住址和照片的学生姓名（多条件筛选和空值处理）
```select stuname from tbstudent where stuaddr is null and stuphoto is null;```

10.查询学生选课的所有日期（去重）
```select distinct(scdate) from tbsc;```

11.查询学生的姓名和生日按年龄从大到小排列（排序）
```select stuname,stubirth from tbstudent order by(stubirth);```

12.查询所有录入了家庭住址的男学生的姓名、出生日期和家庭住址按年龄从小到大排列（多条件筛选和排序）
```select stuname,stubirth,stuaddr from tbstudent where stuaddr is not null and stusex=1 order by(stubirth);```

13.查询年龄最大的学生的出生日期（聚合函数）
```select stuname,stubirth from tbstudent where stubirth=(select min(stubirth) from tbstudent);```

14.查询年龄最小的学生的出生日期（聚合函数）
```select stuname,stubirth from tbstudent where stubirth=(select max(stubirth) from tbstudent);```

15.查询男女学生的人数（分组和聚合函数）
```select stusex,count(stusex) from tbstudent group by(stusex);```

16.查询课程编号为1111的课程的平均成绩（筛选和聚合函数）
```select cid,ifnull(avg(score), 0) from tbsc group by(cid);```

17.查询学号为1001的学生的所有课程的总成绩（筛选和聚合函数）
```select sid,sum(score) from tbsc where sid=1001;```

18.查询每个学生的学号和平均成绩，null值处理为0（分组和聚合函数）
```select stuid,ifnull(c.avg,0) from tbstudent t left join (select sid,avg(tbsc.score) as avg from tbsc group by(sid)) as c on c.sid=t.stuid;```

19.查询平均成绩大于等于90分的学生的学号和平均成绩
```select ts1.stuid,t2.avg from tbstudent ts1,(select sid,avg(score) as avg from tbsc group by(sid)) as t2 where t2.sid=ts1.stuid and t2.avg>=90;```
或者
```select sid,avg(score) as avg from tbsc group by(sid) having avg(score)>=90;```

20.查询年龄最大的学生的姓名
```select t1.stuname from tbstudent t1 where t1.stubirth=(select min(stubirth) from tbstudent);```

21.查询选了两门以上的课程的学生姓名
```select t1.stuname,t1.stuid from tbstudent t1 join (select sid,count(cid) as count from tbsc group by(sid)) as t on t.sid=t1.stuid and t.count>=2;```

22.查询选课学生的姓名和 平均成绩
```select t1.stuname,ifnull(t2.avg,0) from tbstudent t1 join (select sid,avg(score) as avg from tbsc group by(sid)) as t2 on t1.stuid=t2.sid;```

23.查询学生姓名、所选课程名称和成绩
```select t1.stuname,t2.cosname,ifnull(t3.score,0) from tbstudent t1 join tbsc t3 on t1.stuid=t3.sid join tbcourse t2 on t2.cosid=t3.cid;```

24.查询每个学生的姓名和选课数量
```select t1.stuname,ifnull(t2.count,0) from tbstudent t1 left join (select sid,count(cid) as count from tbsc group by(sid)) as t2 on t1.stuid=t2.sid;```

