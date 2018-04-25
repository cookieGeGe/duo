### Django模型之间的关系

Django模型的对应关系，一对一，一对多，以及多对多的关系。

- 一对一 OneToOneField

- 一对多 ForeignKey

- 多对多 ManyToManyField

#### 关联表删除时的操作

on_delete

- protect：当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除。

	models.OneToOneField(other_table, on_delete=models.PROTECT)

- cascade（级联）:当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（不过这就要求该外键允许取null）

	eg:models.OneToOneField(other_table, on_delete=models.CASCADE)

- set_null：父表删除记录时，从表关联字段设置为空

	models.OneToOneField(other_table, on_delete=models.SET_NULL)


- set_default：父表删除记录时，从表设置为默认值

	models.OneToOneField(other_table, on_delete=models.SET_DEFAULT)


### 一对一

一对一模型，顾名思义就是一个唯一对应另一个，比如你的身份证对应你的这种关系，当然也是一个表对应该表的扩展表。这里以用户表和用户信息扩展表做例子。

1）创建用户模型：

```
class User(models.Model):
    u_name = models.CharField(max_length=10)
    u_sex = models.BooleanField()
    u_birth = models.DateField()
    u_create_time = models.DateField(auto_now_add=True)
    u_operate_time = models.DateField(auto_now=True)
    r_id = models.ForeignKey(Role,
                             on_delete=models.CASCADE) #这里指定用户所属的角色组

    class Meta:
        db_table = 'user'
```

2) 创建用户扩展模型：

```
class Userinfo(models.Model):
    u_tel = models.IntegerField()
    u_addr = models.CharField(max_length=50)
    u = models.OneToOneField(User, on_delete=models.CASCADE)

    class Meta:
        db_table = 'info'
```

>**注意：**将两个模型一对一绑定使用的是 ```models.OneToOneField(User, on_delete=models.CASCADE)```
这里会将自动去关联用户模型的主键.

插入数据：

![用户表](https://i.imgur.com/TBJRYqT.png)

![用户拓展表](https://i.imgur.com/oV5CA2W.png)

3）查询用户信息的方法：

```
def selu(request):
    # 拓展表信息查用户信息
    user = Userinfo.objects.get(u_addr='太升路10号').u.u_name

    # 用户信息查拓展信息
    users = User.objects.get(u_name='admin').userinfo
    return render(request, 'show.html', {'users': users, 'user': user})
```

结果如下：

![一对一查询结果](https://i.imgur.com/WlL0kJH.png)

>**注意：**1.通过父表去查询字表的信息，可以通过单个父表用户对象.子模型名的形式得到一个子表对象，通过子表对象即可获取到与附表信息对应的子表的信息。
>2.通过子表信息去查父表对象则通过子表用户对象的外键属性去创建父表用户数据对象，从而获得与之匹配的父表中与该子表信息相关的信息。

### 一对多

一对多指的是在一张表中有多条数据指向另一个表中的某一条数据的这种模式，如一个班级中有多个学生，在学生表中属于同一个班的学生的信息中就有一条指向了班级表中的同一个条目。

这里我们还是以用户表和角色组表来做演示，这里角色组合用户表示一对多的关系，所有应该在用户表中创建外键，管理角色组，在前面我们创建用户模型的时候已经创建了，解下来创建角色模型：

```
class Role(models.Model):
    r_name = models.CharField(max_length=10)

    class Meta:
        db_table = 'role'
```

插入数据：

![角色组](https://i.imgur.com/DNfnS0N.png)

一对多模型下根据角色组查用户和根据用户查属于的角色组：

```
from users.models import User
from authority.models import Role, Permission
def onetomore(request):
    # 查询admin属于哪个角色组
    jiao = User.objects.get(u_name='admin').r_id.r_name

    # 查询管理员角色组下有哪些用户
    users = Role.objects.get(r_name='管理员').user_set.all()

    return render(request, 'show_ontomore.html', {'users': users, 'jiao': jiao})

```

结果如下：

![显示页面配置](https://i.imgur.com/x4iuF8T.png)

![查询结果](https://i.imgur.com/nbkCMp6.png)

>**注意：**在一对多模型中要通过子表访问父表的元素是通过'模型名字_set'去创建父表的对象，来获取父表中的值的。

### 多对多模型

多对多模型指的是一个A和B两张表A表中的某些内容对应B中的多个条目，同时B表中的某些内容又对应A表中的多个条目。例如超市里的商品和购买商品的用户之间的关系，同一个用户可以买很多商品，同一个商品也可以被很多用户购买。

这里在前面的基础上我们以权限表和角色组来作为案例讲解：

1）创建权限模型：

```
class Permission(models.Model):
    p_name = models.CharField(max_length=10)
    p_r = models.ManyToManyField(Role)

    class Meta:
        db_table = 'permission'
```

>**注意：**这里创建权限表使用的是 models.ManyToManyField() 来指定为多对多模型。使用```python manage.py makemigrations```和```python manage.py migrate```创建成功后，可以看到数据库中有三张表，分别是：'permission','role'，'permission_p_r'其中 'permission'
别用来存储 'permission' 和 'role'两张表各个条目的关联关系。'permission_p_r'是Django自动帮我们创建的。

创建后插入如下数据

![权限表](https://i.imgur.com/owbvd0a.png)

![角色组和权限对应表](https://i.imgur.com/bDPugMY.png)

2）数据查询：

```
from authority.models import Role, Permission
def moretomore(request):
    # 查询管理员角色组有哪些权限？
    pers = Role.objects.get(r_name='管理员').permission_set.all()

    # 查询哪些角色组有查询用户列表信息的权限？
    roles = Permission.objects.filter(p_name='查询用户列表信息').first().p_r.all()

    return render(request, 'moretomore.html', {'pers': pers, 'roles': roles})
```
结果如下：

![多对多HTML配置](https://i.imgur.com/iP2JEP7.png)

![多对多结果页面](https://i.imgur.com/GnmGdPL.png)

>**注意：**这里在做对象转换的时候分别使用的是```permission_set```和```p_r```,在实际使用的时候要注意这里的方法分别为 'A表_set' 和 第三张表用下划线连起来的后两个字段。

扩展练习：

1) 查询admin用户具备那些权限？

2）判断王大锤一个用户是否有登录权限？
```
def showall(request):
    #1) 查询admin用户具备那些权限？

    pers = User.objects.get(u_name='admin').r_id.permission_set.all()

    #2）判断王大锤一个用户是否有登录权限？

    islogin = User.objects.get(u_name='王大锤').r_id.permission_set.filter(p_name='登录').exists()

    return render(request, 'showall.html', {'pers': pers, 'islogin': islogin})
```

结果如下图：

![显示界面配置](https://i.imgur.com/Ogppqoq.png)

![结果](https://i.imgur.com/dwBRUrX.png)