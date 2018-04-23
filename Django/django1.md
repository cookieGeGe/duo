## Django视图和模型

### 创建项目helloworld1

使用 django-admin 来创建 helloworld1 项目：

```django-admin startproject helloworld1```

创建完成后我们可以查看下项目的目录结构：

![目录结构](https://i.imgur.com/Nw06Muv.png)

目录说明：

- helloworld1: 项目的容器。

- manage.py: 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。

- helloworld1/__init__.py: 一个空文件，告诉 Python 该目录是一个 Python 包。

- helloworld1/settings.py: 该 Django 项目的设置/配置。

- helloworld1/urls.py: 该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。

- helloworld1/wsgi.py: 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

### 字符编码和时区设置

对项目的设置都在helloworld1/settings.py中

设置中文字符编码，将LANGUAGE_CODE字段的'en-us'改为'zh-hans'：

```LANGUAGE_CODE = 'zh-hans'```

设置时区，TIME_ZONE字段的'UTC'改为'Asia/Shanghai'

```TIME_ZONE = 'Asia/Shanghai'```

### 视图和URL设置

Django是MTV设计模式，在Django中所有来自用户的请求都是先交由url分发器路由给对应的view处理后再根据module和template返回的结果返回给用户渲染后的页面。

在项目名下的urls.py中就存储了这些路由信息，默认情况下/helloworld1/urls.py中的信息如下：

![urls默认配置](https://i.imgur.com/0lC1bZL.png)

默认配置中存储了一条到管理员后台登录页面的路由，在启动服务器的情况下我们可以输入127.0.0.1:8000/admin访问登录页面，结果如下图：

![后台登录页面](https://i.imgur.com/4Wqt9H0.png)

### 创建视图和url路由

在 /helloworld/ 目录下创建视图文件view.py，并在输入代码：

```
from django.http import HttpResponse
 
def hello(request):
    return HttpResponse("Hello world ! ")

```

接着为创建的视图绑定url路由，编辑 /helloworld/ 目录下的urls.py的内容为：

```
from django.conf.urls import url
from django.contrib import admin
from . import view

urlpatterns = [
    url(r'^hello/', view.hello),
    url(r'^admin/', admin.site.urls),
]
```

目录结构为：

![添加hello视图后的目录结构](https://i.imgur.com/2hpJegj.png)

这样当用户请求为127.0.0.1:8000/hello/时，就会返回视图中定义好的 'hello world!',结果如下图：

![127.0.0.1:8000/hello/](https://i.imgur.com/BD6JS3P.png)

到这里应该就能够理解到Django中urls路由是怎么处理用户的请求的了吧。

#### url()函数

Django url() 可以接收四个参数，分别是两个必选参数：regex、view 和两个可选参数：kwargs、name，接下来详细介绍这四个参数。

- regex: 正则表达式，与之匹配的 URL 会执行对应的第二个参数 view。

- view: 用于执行与正则表达式匹配的 URL 请求。

- kwargs: 视图使用的字典类型的参数。

- name: 用来反向获取 URL。


#### Django连接数据库

MySQL是web应用中最常用的数据库，在web应用中要做到数据持久化就必须要用到数据库。那么Django怎么连接到数据库呢？在Django的settings.py中就有定义数据库的字段，找到DATABASES字段即可完成对数据库的设置。

这里以连接我本地虚拟机中的mariadb为例：
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'helloworld',
        'HOST': '192.168.80.139',
        'USER': 'root',
        'PASSWORD': '123456',
        'PORT': '3306',
    }
}
```
上面数据库的名称、用户、密码、以及地址与我本地的 mariadb 中的信息吻合，Django 经过这么设置后 mariadb 中相应的数据库就和用户连接起来了。

### 定义模型

#### 创建APP

Django 规定，Django 规定，如果要使用模型，必须要创建一个app。这里我们创建一个APP来，然后来创建表并往表里面插入一些数据来展示一下APP和连接数据的效果。

创建APP命令：

```django-admin startapp app_name```

我这里创建一个名为test1的APP，创建后机构如下图：

![创建test1后的目录结构](https://i.imgur.com/stHxgmS.png)

在 python3 中连接mysql使用的库是 pymysql ，需要在虚拟环境中安装，而且在 python3 中 Django 连接数据库，还需要编辑 APP 的 __init__.py 的初始化配置文件，使其兼容，否则在启动服务器的时候会提示你安装 mysqlclient 。在APP的初始化配置文件中添加代码：

```
import pymysql

pymysql.install_as_MySQLdb()
```

接下来修改 APP 下的 models.py 文件创建数据表。代码如下：

```
from django.db import models


class Test(models.Model):
    name = models.CharField(max_length=20)
    gender = models.BooleanField
    addr = models.CharField(max_length=50)

    class Meta:   # 元类
        db_table = 'people_msg' # 定义创建的表的名字
```

以上的类名代表了数据库表名，且继承了 models.Model ，类里面的字段代表数据表中的字段(name)，数据类型则由CharField（相当于varchar）、DateField（相当于datetime）， max_length 参数限定长度。

接下来需要在项目的配置文件中添加 APP信息，修改 settings.py 中的INSTALLED_APPS的信息，添加我们创建的APP，代码如下：

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'test1'
]
```

接着是迁移数据库，需要在命令行中执行：

```
python manage.py makemigrations

python manage.py migrate  # 创建表结构
```

>**注意：**在 models.py 文件中创建表的时候，如果我们没有指定表名，那么表名的命名规则将是应用名_类名；在创建表的时候如果没有指定主键，Django会自动添加一个id作为主键。

迁移完数据库后数据库中的表结构为：

![创建表过程](https://i.imgur.com/1e1hplT.png)

![mariadb中的表结构](https://i.imgur.com/rDFwt1g.png)

到这里我们就可以看到数据库已经和Django连接成功了。

### 创建视图和url路由对数据库中数据进行操作

#### 插入数据

在APP下的 views.py 定义插入数据函数 insertdb,插入相应数据。代码如下：

```
def insertdb(request):
    
    for i in ('张三', '李四', '王大锤'):
        people = Test()
        people.name = i
        people.gender = 1
        people.addr = '四川成都'
        people.save()

    return HttpResponse("<h1>添加数据成功！</h1>")

```


#### 读取数据

在APP下的 views.py 定义插入数据函数 selectdb,插入相应数据。代码如下：

```
def selectdb(request):
    response1 = ""

    # 通过objects这个模型管理器的all()获得所有数据行，相当于SQL中的SELECT * FROM
    list1 = Test.objects.all()

    # 输出所有数据
    for var in list1:
        response1 += var.name + " " + var.addr + " \n"
    response = response1
    return HttpResponse("<p>" + response + "</p>")
```

#### 更新数据

在APP下的 views.py 定义插入数据函数 updatedb,插入相应数据。代码如下：

```
def updatedb(request):
    test1 = Test.objects.get(id=1)
    test1.name = 'Google'
    test1.save()
    return HttpResponse("<p>ID为1的名字修改成功</p>")
```

#### 删除数据

在APP下的 views.py 定义插入数据函数 deletetb,插入相应数据。代码如下：

```
def deletetb(request):
    test1 = Test.objects.get(id=2)
    test1.delete()
    return HttpResponse("<p>成功删除ID为2的行</p>")
```

### 为以上四个操作添加url路由

在APP目录下新建 urls.py,添加代码如下：

```
from test1 import views
from django.conf.urls import url

urlpatterns = [
    url(r'insert/', views.insertdb),
    url(r'select/', views.selectdb),
    url(r'update/', views.updatedb),
    url(r'delete/', views.deletetb),
]

```

修改项目目录下的 urls.py文件为如下形式：

```
from django.conf.urls import url,include
from django.contrib import admin
from . import view

urlpatterns = [
    url(r'^hello/', view.hello),
    url(r'^admin/', admin.site.urls),
    url(r'^test1/', include('test1.urls'))
]
```

插入数据后的数据表：

![成功添加数据](https://i.imgur.com/XKasyFp.png)

查询出来的数据表：

![查询结果](https://i.imgur.com/xIYVPQ1.png)

更新后的数据表：

![更新数据表结果](https://i.imgur.com/IpmEhpa.png)

删除数据后的数据表：

![删除数据表的结果](https://i.imgur.com/YQYSSeg.png)