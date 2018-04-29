## Django url路由系统进阶
<hr>

django的路由系统作用就是使views里面处理数据的函数与请求的url建立映射关系。使请求到来之后，根据urls.py里的关系条目，去查找到与请求对应的处理方法，从而返回给客户端http页面数据

![url路由系统工作图](https://i.imgur.com/po3woMJ.png)

django 项目中的url规则定义放在project 的urls.py目录下，
默认如下：

```
from django.conf.urls import url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
]
```

url()函数可以传递4个参数，其中2个是必须的：regex和view，以及2个可选的参数：kwargs和name。下面是具体的解释：

- regex：
regex是正则表达式的通用缩写，它是一种匹配字符串或url地址的语法。Django拿着用户请求的url地址，在urls.py文件中对urlpatterns列表中的每一项条目从头开始进行逐一对比，一旦遇到匹配项，立即执行该条目映射的视图函数或二级路由，其后的条目将不再继续匹配。因此，url路由的编写顺序至关重要！
需要注意的是，regex不会去匹配GET或POST参数或域名，例如对于https://www.example.com/myapp/，regex只尝试匹配myapp/。对于https://www.example.com/myapp/?page=3,regex也只尝试匹配myapp/。

- view：
当正则表达式匹配到某个条目时，自动将封装的HttpRequest对象作为第一个参数，正则表达式“捕获”到的值作为第二个参数，传递给该条目指定的视图。如果是简单捕获，那么捕获值将作为一个位置参数进行传递，如果是命名捕获，那么将作为关键字参数进行传递。

- kwargs：
任意数量的关键字参数可以作为一个字典传递给目标视图。

- name：
对你的URL进行命名，可以让你能够在Django的任意处，尤其是模板内显式地引用它。相当于给URL取了个全局变量名，你只需要修改这个全局变量的值，在整个Django中引用它的地方也将同样获得改变。这是极为古老、朴素和有用的设计思想，而且这种思想无处不在。

### url传递参数

#### url传递单个参数

利用url传递参数可以帮我们解决很多的问题，比如从一个页面跳转到一个详情页面的时候就需要给后一个页面传递一个参数才能实现这种功能。这里举一个简单的例子，来理解是如何通过url传递参数，和获取参数。

如果要通过url传递参数，就需要使用正则表达式去匹配参数，并且在视图函数中必须去接收这个参数（如果不接收参数，就会报错）。这里以传递一个参数，并显示这个参数为例：

url路由：

```
url(r'^onearg/(\w+)/$', views.showarg)
```

视图处理函数代码：

```
from django.http import HttpResponse


def showarg(request, arg):
    return HttpResponse('URL中包含的参数是%s' % (arg))
```

测试结果：

![URL中传递一个参数的项目配置和结果](https://i.imgur.com/wlWTgVe.png)

#### URL传递多个参数

就像上面说的那样，既然URL可以传递一个参数，自然也可以传递多个参数，传递多个参数的时候，默认会以传入的次序依次传递给视图函数中的形式参数，也可以使用正则表达式的分组命名的方式传入命名参数。这里我们以传入命名参数为例，利用URL传递两个数字，并返回两个数字的和。

url路由信息：

```
url(r'morearg/(?P<arg1>\d+)/(?P<arg2>\d+)/$', views.showmore)
```

视图处理函数：

```
def showmore(request, arg2, arg1):
    sum1 = int(arg1) + int(arg2)
    return HttpResponse(
        '<p>URL传递的参数arg1是：%s</p><p>URL传递的参数arg2是：%s</p>URL中参数的和是:%s' % (arg1, arg2, sum1)
    )
```

测试结果:

![URL传参求和](https://i.imgur.com/pelOuQA.png)

## 页面get传参

演示页面get传参需要绑定数据库，创建学生和班级模型，我们写一个所有班级的页面点班级，查看该班级下所有的学生：

配置项目目录下的urls.py，为两个模型创建路由，和命名空间：

```
from django.conf.urls import url, include
from django.contrib import admin
from . import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^onearg/(\w+)/', views.showarg),
    url(r'morearg/(?P<arg1>\d+)/(?P<arg2>\d+)/', views.showmore),
    url(r'^stu/', include('stu.urls', namespace='s')),
    url(r'^grade/', include('grade.urls', namespace='g')),
]
```


班级模型Grade相关配置：

配置grade/urls.py，为访问所有班级页面添加url路由:

```
from django.conf.urls import url
from grade import views

urlpatterns = [
    url(r'^allg/', views.allg, name='ag'),
]
```

配置grade/views.py，为为访问所有班级页面添加视图函数:
```
def allg(requset):
    allgs = Grade.objects.all()
    return render(requset, 'allg.html', {'allg': allgs})
```

学生模型Student相关配置:

编辑stu/urls.py显示某班级下的所有学生添加URL路由:

```
from django.conf.urls import url
from stu import views

urlpatterns = [
    url(r'^allstu/', views.showall, name='as'),
]
```

编辑stu/views.py,为显示某班级下的所有学生添加视图函数:

```
from django.shortcuts import render
from stu.models import Student

def showall(request):
    gid = request.GET.get('gid')
    stus = Student.objects.filter(g_id=gid)
    return render(request, 'g_stu.html', {'stus': stus})
```

HTML模板页面：

配置一个继承模板stus.html:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}

        {% endblock %}
    </title>
</head>
<body>
{% block contain %}

{% endblock %}
</body>
</html>
```

从模板继承，生成显示所有班级的页面allg.html:

```
<title>
    {% block title %}
        所有班级信息
    {% endblock %}
</title>
{% block contain %}
    {% for gra in allg %}
        <p>班级ID：<a href="/stu/allstu/?gid={{ gra.id }}">{{ gra.g_id }}</a></p>
        <p>班级名称：{{ gra.g_name }}</p>
        <p>班级描述：{{ gra.g_desc }}</p>
        <hr />
    {% endfor %}
{% endblock %}
{% include 'stus.html' %}
```

从模板继承，生成显示班级学生的页面g_stu.html:

```

<head>
<title>
    {% block title %}
        {{ gname }}班级详情
    {% endblock %}

</title>
</head>
{% block contain %}
    {% for stu in stus %}
        <p>姓名：{{ stu.s_name }}</p>
        <p>电话：{{ stu.s_tel }}</p>

        <hr />
    {% endfor %}
    <a href="/grade/allg/">返回班级页面</a>
{% endblock %}
{% include 'stus.html' %}
```

预览页面：

![所有班级](https://i.imgur.com/2KeTt4H.png)

![跳转页面](https://i.imgur.com/2d3CaQm.png)

这种get传参的方式只能是通过页面内去指定的方式来传递参数。形如：```<a href="/stu/allstu/?gid={{ gra.id }}">{{ gra.g_id }}</a>```

## 反向解析

随着功能的增加会出现更多的视图，可能之前配置的正则表达式不够准确，于是就要修改正则表达式，但是正则表达式一旦修改了，之前所有对应的超链接都要修改，真是一件麻烦的事情，而且可能还会漏掉一些超链接忘记修改，有办法让链接根据正则表达式动态生成吗？ 就是用反向解析的办法。

反向解析主要应用在两个方面，一是模板中的超链接，二是视图中的重定向。如果要使用反向解析，在定义url时，需要为include定义namespace属性，为url定义name属性，在模板中使用url标签，在视图中使用reverse函数，根据正则表达式动态生成地址，减轻后期维护成本。

#### 在模板中使用反向解析：

延用前面的例子，我们已经为创建了命名空间和name,这里只需要修改一部分信息即可实现在模板中的反向解析，相关配置如下：

stu/urls.py:

```
from django.conf.urls import url
from stu import views

urlpatterns = [
    url(r'^allstu/(\d+)/', views.showall, name='as'),
]
```

stu/views.py:

```
from django.shortcuts import render
from stu.models import Student


# Create your views here.

def showall(request, gid):
    stus = Student.objects.filter(g_id=gid)
    gname = stus.first().g_id.g_name
    return render(request, 'g_stu.html', {'stus': stus, 'gname': gname})
```

templates/g_stu.html:
```
<title>
    {% block title %}
        所有班级信息
    {% endblock %}
</title>
{% block contain %}
    {% for gra in allg %}
        <p>班级ID：<a href="{% url 's:as' gra.id %}">{{ gra.g_id }}</a></p>
        <p>班级名称：{{ gra.g_name }}</p>
        <p>班级描述：{{ gra.g_desc }}</p>
        <hr />
    {% endfor %}
{% endblock %}
{% include 'stus.html' %}

```

预览页面：

![预览1](https://i.imgur.com/jsUy2ak.png)

![模板反向解析](https://i.imgur.com/pFpocRY.png)


### 重定向

URL重定向（英语：URL redirection，或称网址重定向或网域名称转址），是指当用户浏览某个网址时，将他导向到另一个网址的技术。常用在把一串很长的网站网址，转成较短的网址。因为当要传播某网站的网址时，常常因为网址太长，不好记忆；又有可能因为换了网络的免费网页空间，网址又必须要变更，不知情的用户可能会认为网站关闭了。这时就可以用网络上的转址服务了。这个技术使一个网页是可借由不同的统一资源定位符（URL）链接。

继续以上示例，接下来以添加用户跳转到添加用户扩展信息然后跳转到所有班级页面作为案例：

添加学生拓展信息表(stu/models.py)：
```
class Stuinfo(models.Model):
    s_addr = models.CharField(max_length=50)
    sid = models.OneToOneField(Student, null=True)

    class Meta:
        db_table = 'stuinfo'
```

为添加学生、和添加学生拓展信息页面添加URL路由(stu/urls.py)：

```
url(r'^addstu/', views.addstu),
url(r'^addinfo/(?P<stuid>\d+)/', views.addinfo, name='ainfo'),
```

为添加学生和添加学生拓展信息页面添加view视图函数(stu/views.py):

```
def addstu(request):
    if request.method == 'GET':
        return render(request, 'addstu.html')
    if request.method == 'POST':
        name = request.POST.get('name')
        tel = request.POST.get('tel')
        gid = request.POST.get('gid')
        gid = Grade.objects.get(id=gid)
        stu = Student.objects.create(
            s_name=name,
            s_tel=tel,
            g_id=gid,
        )

        return HttpResponseRedirect(
            reverse('s:ainfo', kwargs={'stuid': stu.id})
        )


def addinfo(request, stuid):
    if request.method == 'GET':
        return render(request, 'addinfo.html')
    if request.method == 'POST':
        addr = request.POST.get('addr')
        sid = Student.objects.get(id=stuid)

        Stuinfo.objects.create(
            s_addr=addr,
            sid=sid,
        )
        return HttpResponseRedirect('/grade/allg/')
```

>**注意：**跳转到添加学生拓展信息页面的url中的正则匹配的组名必须和重定向中kwargs字典中的键匹配。

添加学生信息（templates/addstu.html）:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加学生</title>
</head>
<body>
<form action="" method="post">
    <p>姓名：<input type="text" name="name"></p>
    <p>电话：<input type="text" name="tel"></p>
    <p>班级ID：<input type="text" name="gid"></p>
    <input type="submit" value="提交">
</form>
</body>
</html>
```
添加学生拓展信息（templates/addinfo.html）:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加学生</title>
</head>
<body>
<form action="" method="post">
    <p>地址：<input type="text" name="addr"></p>
    <input type="hidden" value="{{ stuid }}" name="sid">
    <input type="submit" value="提交">
</form>
</body>
</html>
```

测试结果如下：

![添加学生信息](https://i.imgur.com/LKgK2Eq.png)

![添加学生拓展信息](https://i.imgur.com/eDOEsAJ.png)

![班级页面信息](https://i.imgur.com/svCp4QA.png)

![拓展信息表信息](https://i.imgur.com/iuJTTIg.png)

## 404/500错误重定向

开发都遵循用户最小惊讶原则，所以我们在开发的时候需要将404和500错误进行重定向。对404和500进行错误重定向需要提前关闭Django的DEBUG功能，并且设置为运行所有主机访问。

修改项目目录下的settings.py文件：

```
DEBUG = False
ALLOWED_HOSTS = ['*']
```
对于错误重定向，不需要做url路由，直接写错误处理视图函数，然后在项目urls.py下设置调用即可。

这里为了演示方便直接写在项目目录下的views.py中：

```
def page_not_found(request):
    return HttpResponse('<h1>404错误</p>')


def server_error(request):
    return HttpResponse('<p>500服务器内部错误</p>')
```

项目目录下的urls.py文件配置：
```
from .views import page_not_found,server_error
hander404 = page_not_found

hander500 = server_error
```


