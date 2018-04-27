### Django后台管理

在前一篇中最后我们大致了解了一下 Django 如何向数据库中添加数据，当然这只是为了演示 Django 中的模型。 Django 也提供了很好的基于web的后台管理。

在创建好Django项目之后，在项目目录下的 urls.py 路由配置文件中有一行 ```url(r'^admin/', admin.site.urls)``` 这里就配置了Django后台管理的入口。启动服务在浏览器中打开 http://127.0.0.1:8000/admin/就可以看到如下图的后天登录页面：

![后台登录页面](https://i.imgur.com/pbetMid.png)

你可以在命令行中使用 ```python manage.py createsuperuser``` 命令来创建登录 Django 后台的超级管理员账号，登录后的界面如下：

![登录成功界面](https://i.imgur.com/99H7yjJ.png)

>**注意：**修改用户密码使用：```python manage.py changepassword username```

这里默认是没有任何数据模型的，如果要通过Django后台去管理某个数据模型，还需要注册该数据模型到 admin 中。这里以注册 stu 模型为例，去修改 stu 模型下的 admin.py 来实现注册，代码如下：

```
from django.contrib import admin

from stu.models import Student

admin.site.register(Student)
```

![添加stu模型](https://i.imgur.com/jwwRsR8.png)

![添加用户](https://i.imgur.com/KtFk7Ju.png)

到这里我们就可以通过Django给我们提供的后台管理向数据表里面添加数据了，只是添加数据后返回的是一个对象。

### 修改后台管理页面输出界面

为什么需要修改默认后台管理页面呢？因为默认的页面非常的丑而且显示的信息并不是表里面的信息，而是一个个的对象。如下图：

![默认](https://i.imgur.com/ZvYrZHX.png)

上图中的Student object就是我们创建的学生对象，可是这样显示的结果我们什么都看不到，所以我们就需要修改模型下的 admin.py 文件，让输出的信息是我们能看到哦的信息。

这里以我创建的为例，修改页面支持内容：

- 通过用户名搜索

- 分页显示，每页5个

- 根据ID号排序

- 性别显示为男或者女，而不是布尔值

在 admin.py 文件中添加如下代码：

```
class StudentAdmin(admin.ModelAdmin):
    def set_sex(self):
        if self.sex:
            return '男'
        else:
            return '女'

    def set_name(self):
        return self.name

    # 修改描述
    set_sex.short_description = '性别'
    set_name.short_description = '姓名'

    # 过滤
    list_filter = ['name']

    # 排序
    ordering = ['id']

    # 搜索
    search_fields = ['name']

    # 分页
    list_per_page = 5

    list_display = ['id', set_name, set_sex]
```

结果如图：

![修改后的效果图](https://i.imgur.com/C3TqsN8.png)

### 补充

1.除了通过上面的方法注册模型外还可以使用装饰器的方式注册模型。使用装饰器注册代码为 ```@admin.register(Student)```。

2.注册模型的时候可以同时注册多个模型，多个模型名称用 ',' 隔开即可。如：admin.site.register(Student, StudentAdmin)

