### Django ORM详解

#### 什么是ORM？

ORM，即Object-Relational Mapping（对象关系映射），它的作用是在关系型数据库和业务实体对象之间作一个映射，这样，我们在具体的操作业务对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作对象的属性和方法。


图解：

![ORM图解](https://i.imgur.com/iONt6Zr.png)

相信学过linux运维，或者做运维的小伙伴都知道，在linux系统中有一个叫做 VFS（虚拟文件系统）的东西，VFS 是干什么用的呢？ VFS就是为不同的文件系统提供了一个统一的操作界面和应用编程接口。我个人觉得这里的ORM类似于VFS，至少处理问题的方式是类似的。

#### ORM中操作数据库

本质上来说ORM操作数据库也不过是进行CRUD的操作。只是在ORM中将原生的SQL语句都封装成了方法或者函数，方便我们快捷调用。下面来详细介绍这些方法或函数：

1)模型成员objects

Django默认通过模型的objects对象实现模型数据查询

2）数据查询和过滤

过滤实际上就相当于SQL语句中的where语句，和过滤相关的方法或函数如下：

- all() : 返回所有的数据，得到的是一个列表

- filter() ：返回复合条件的数据，条件可以有多个，多个条件用 ',' 隔开，得到的结果也是一个列表

- exclude() ：过滤掉符合条件的数据，返回一个列表

-   order_by() : 按照指定字段进行排序

- values() : 每条数据都作为一个字典，放在一个列表中

- get() : 也可以接受多个条件，与 filter 不同，这里返回的是一个满足条件的对象，为单个值，且如果没有符合条件的就会报MultiObjectReturned错误，而 filter 不会报错，而是返回一个空列表。

- first()：返回查询结果中的第一个对象

- last()：返回查询结果中的最后一个对象

- count()：返回当前查询结果中的对象个数

- exists()：判断查询结果中是否有数据，如果有数据返回True，没有返回False

3) 限制查询结果的条数

限制结果的条数，类似于SQL中的limit和offset。在ORM中则是使用下标的方式去限制，这里有点类似于 python 中列表的下标运算，只是这里的下标不能为负数。

如：模型名.objects.all()[0:5]

4)ORM中的模糊查询

- startswith : 匹配开始字符串

- endswith : 匹配结束字符串

- contains : 匹配包含该字符串的项

示例：

```
查询姓王的数据
stus = Student.objects.filter(stu_name__startswith='王')
查询以什么结束的数据：
stus = Student.objects.filter(stu_name__endswith='胖')
查询姓名中包含大的数据
stus = Student.objects.filter(stu_name__contains='大')
```

5)ORM中的比较运算符

- isnull，isnotnull：是否为空。
	如：filter(stu_name__isnull=True)

- in：是否包含在范围内。这个类似python中的in
	如：filter(id__in=[1,2,3])

- gt，gte，lt，lte：大于，大于等于，小于，小于等于。
	如：filter(age__gt=10)

- pk：代表主键，也就是id。

6)聚合函数

与SQL中的聚合函数一样，只是ORM中药通过ggregate()函数获得聚合函数的返回值。

- Avg：求平均值

- Count：求次数

- Max：求最大值

- Min：求最小值

- Sum：求和

例如: Student.objects.aggregate(Max('age'))

7)F对象和Q对象

导入F对象和Q对象： ```from django.db.models import F, Q```

- F对象：使用在模型的A属性与B属性进行比较的时候，比如要比较一个学生的A成绩和B成绩的时候就需要用到F对象。

例如：查找语文成绩比数学成绩至少高10分的学生：

```stus = Student.objects.filter(stu_yuwen__gte=F('stu_shuxue') + 10)```

- Q对象：是为了将过滤条件组合起来查询，在查询的条件中需要组合条件 “且（&）” 或者 “或（ | ）” 时。我们需要使用Q()查询对象。

例如：查询学生姓名不叫王大锤的，或者语文成绩大于80分的学生

```
stus = Student.objects.filter(~Q(stu_name='王大锤') | Q(stu_yuwen__gt=80))
```
>注意：Q()对象的前面使用字符“~”来代表意义“非”

### 模型字段定义

##### 库

定义属性时，需要字段类型，字段类型被定义在django.db.models.fields目录下，为了方便使用，被导入到django.db.models中，所以在定义属性时要导入库，并通过models去定义属性。

导入库：APP目录下的models.py文件中

```
from django.db import models
```

定义属性：

```
class Student(models.Model):
	stu_name = models.CharField(max_length=10)
```

##### 属性命名规则

- 遵循标识符规则(不使用python预定义的标识符号，内置函数名，异常等。避免使用下划线等)

- 由于django的查询方式，不允许使用连续的下划线


##### 字段类型

- AutoField() ： 一个根据实际ID自动增长的IntegerField，通常不指定如果不指定，一个主键字段将自动添加到模型中.

- CharField(max_length=字符长度,....) ： 字符串，默认的表单样式是 TextInput.

- TextField() : 大文本字段，一般超过4000使用，默认的表单控件是Textarea.

- IntegerField() : 整数

- DecimalField(max_digits=None, decimal_places=None) : 使用python的Decimal实例表示的十进制浮点数, max_digits 指定总位数，decimal_places 指定小数点后保留几位。

- FloatField() : 用Python的float实例来表示的浮点数.

- BooleanField() : true/false 字段，此字段的默认表单控制是CheckboxInput.

- NullBooleanField() : 支持null、true、false三种值.

- DateField([auto_now=False, auto_now_add=False]) : 使用Python的datetime.date实例表示的日期 auto_now 每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为false。 auto_now_add 当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为false.

>**说明:**该字段默认对应的表单控件是一个TextInput. 在管理员站点添加了一个JavaScript写的日历控件，和一个“Today"的快捷按钮，包含了一个额外的invalid_date错误消息键。

>**注意：**auto_now_add, auto_now, and default 这些设置是相互排斥的，他们之间的任何组合将会发生错误的结果。

- TimeField() : 使用Python的datetime.time实例表示的时间，参数同DateField.

- DateTimeField() : 使用Python的datetime.datetime实例表示的日期和时间，参数同DateField.

- FileField() : 一个上传文件的字段.

- ImageField : 继承了FileField的所有属性和方法，但对上传的对象进行校验，确保它是个有效的image.

##### 字段选项

概述

通过字段选项，可以实现对字段的约束,在字段对象时通过关键字参数指定.

- null : 如果为True，则该字段在数据库中是空数据，默认值是 False

- blank : 如果为True，则该字段允许为空白，默认值是 False

>**注意:** null 是数据库范畴的概念，blank 是表单验证证范畴的

- db_column : 字段的名称，如果未指定，则使用属性的名称

- db_index : 若值为 True, 则在表中会为此字段创建索引

- default : 默认值

- primary_key : 若为 True, 则该字段会成为模型的主键字段

>django会为表增加自动增长的主键列（ID），每个模型只能有一个主键列，如果使用选项设置某属性为主键列后，则django不会再生成默认的主键列。

- unique : 如果为 True, 这个字段在表中必须有唯一值

#### 补充

逻辑删除

对公司来说最重要的就是数据，删除数据的操作都是不可恢复的。所以在实际开发可能会对不需要的书进行删除，这种情况我们通常都是对数据做逻辑删除，不做物理删除，防止删除数据后，某些时候又需要该数据不能找回。那么什么是逻辑删除呢？逻辑删除就是在数据表中创建一个字段，这个字段通常是BooleanField，用True表示该数据有效，用False表示该数据无效被删除了。


>参考自：[https://github.com/coco369/knowledges/blob/master/django/2.1django_models.md](https://github.com/coco369/knowledges/blob/master/django/2.1django_models.md)