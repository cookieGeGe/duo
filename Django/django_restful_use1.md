## Django - RESTful使用指南

按照 Django 的常规方法当然也可以实现REST，但有一种更快捷、强大的方法，那就是 Django REST framework.它是python的一个模块，通过在 Django 里面配置就可以把 app 的 models 中的各个表实现 RESTful API。具体实现方法如下：

### 安装配置
	# 实现restful的模块
	pip install djangorestframework==3.4.6
	# 实现restful过滤功能的模块
	pip install django-filter

>注意：这里使用的是3.4.6版本的 djangorestframework ，不同版本之间的 djangorestframework 有所不同，可以故意打错版本号来查看 djangoframework 来查看有哪些版本。

安装完毕以上两个模块后，还需要将 rest_framework 作为 APP 添加到 settings.py 文件中。

```
INSTALLED_APPS = [
    ....
    'rest_framework',
]
```

### 创建model

在 stu APP中创建两张表，学生表和学生拓展信息表：

	from django.db import models
	
	
	# Create your models here.
	
	
	class Student(models.Model):
	    s_name = models.CharField(max_length=10)
	    s_tel = models.CharField(max_length=11)
	
	    class Meta:
	        db_table = 'stu'
	
	
	class StudentInfo(models.Model):
	    i_addr = models.CharField(max_length=50)
	    sid = models.OneToOneField(Student)
	
	    class Meta:
	        db_table = 'stu_info'

### 创建序列化类（Serializer）

创建序列化类的作用就是从你传入的参数中提取出你需要的数据，并把它转化为 json 格式返回。这里在stu文件夹下创建serializers.py，并添加如下内容：

	from rest_framework import serializers
	from stu.models import Student
	
	
	class studentserializer(serializers.ModelSerializer):
	    class Meta:
	        model = Student
	        fields = ['id', 's_name', 's_tel']
	
	    def to_representation(self, instance):
	        data = super().to_representation(instance)
	        try:
	            data['i_addr'] = instance.studentinfo.i_addr
	        except:
	            data['i_addr'] = ''
	        return data


### 添加url路由

创建一个能够获取学生模型中数据的 API 接口路由：

	from django.conf.urls import url
	from rest_framework.routers import SimpleRouter
	
	from stu import views

	# 创建一个一个Simplerouter对象
	routers = SimpleRouter()

	# 注册获取数据的API接口路由，执行的视图为'views.showallstu'
	routers.register(r'student', views.showallstu)
	urlpatterns = []

	# 将注册路由添加到django路由系统中
	urlpatterns += routers.urls

### url视图处理

对于普通的url请求我们是在views.py中定义的一个函数，但是对于RESTful是定义一个类，定义的showallstu类如下:

	from django.shortcuts import render
	from rest_framework import mixins, viewsets
	
	# Create your views here.
	from stu.models import Student
	from stu.serializers import studentserializer
	
	
	class showallstu(mixins.CreateModelMixin,  # 用于创建数据
	                 mixins.DestroyModelMixin,  # 用于删除数据
	                 mixins.UpdateModelMixin,	# 用于更新数据
	                 mixins.RetrieveModelMixin, # 用于显示单个数据
	                 mixins.ListModelMixin,	# 用于显示所有数据
	                 viewsets.GenericViewSet):
		# 设置要返回的内容queryset
	    queryset = Student.objects.all()
		# 指定序列化类，将返回的内容序列化为json格式	    
		serializer_class = studentserializer

到这里一个支持对学生表进行增、删、查、改的数据接口就做好了，我们可以使用接口调试神器[POSTMAN](https://www.getpostman.com/)来进行测试。

结果如下图：

![postman调试结果](https://i.imgur.com/JhBYvNq.png)

<hr />

## 其他

### 自定义返回的json格式

通常对于一个请求，无论是否成功，我们都应该返回一些东西来告知其请求的结果，对于上面的这种如果没有数据，那么返回的便是一个空列表，因此我们需要自定义JSONRenderer。

自定义JSONRenderer的方法很简单，通过创建一个类去继承JSONRenderer，并重构其render方法，然后在settings.py文件中修改默认使用的renderer类为我们自定义的类即可。

在项目目录下创建文件夹utils，在utils目录下创建rendererresponse.py文件，添加如下代码自定义返回的json数据：
	
	# 导入控制返回的JSON格式的类
	from rest_framework.renderers import JSONRenderer
	
	
	class customrenderer(JSONRenderer):
		# 重构render方法
	    def render(self, data, accepted_media_type=None, renderer_context=None):
	        if renderer_context:
				# 获取需要返回的msg和code信息，没有则赋值
	            if isinstance(data, dict):
	                msg = data.pop('msg', '请求成功')
	                code = data.pop('code', 0)
	            else:
	                msg = '请求成功'
	                code = 0
				# 重新构建返回的JSON字典
	            ret = {
	                'msg': msg,
	                'code': code,
	                'data': data,
	            }
				# 返回JSON数据
	            return super().render(ret, accepted_media_type, renderer_context)
	        else:
	            return super().render(data, accepted_media_type, renderer_context)

settings.py文件中，修改默认renderer类：

	REST_FRAMEWORK = {
	
	    # 修改默认返回JSON的renderer的类
	    'DEFAULT_RENDERER_CLASSES': (
	        'utils.rendererresponse.customrenderer',
	    ),
	}

返回的json数据格式为：

![自定义返回JSON格式](https://i.imgur.com/sEfXXff.png)


### 异常处理

自定义异常处理，一定需要继承from rest_framework.exceptions import APIException 中的APIException，在编写自己的异常处理的方法：

![自定义异常处理](https://i.imgur.com/qbqSoMf.png)

### 空值处理

除了对异常错误的处理之外，我们还要对传入数据是否是空值做处理。因为传入空值服务器会返回错误，如下图：

![空值报错](https://i.imgur.com/29Jo2RM.png)

如上图，如果传入的数据时空值的话，那么服务器就会返回一个 "This field may not be blank." 的报错。这时候我们需要在serializer中定义s_name的序列化，指定错误的信息，为空的话，提示响应的错误信息。

![空值自定义报错](https://i.imgur.com/RiOF2SD.png)

### 实现分页

通常用户获取的数据可能是有很多条的，我们是不可能所有的数据一起返回给用户的，明显那是不理智的，所以我们还需要对返回的数据做分页处理。

用 restful 实现分页很简单，只需要在settings.py中设置默认分页的类和分页的每页显示的条数即可：

	REST_FRAMEWORK = {
	
	    # 自定义返回的json格式
	    'DEFAULT_RENDERER_CLASSES': (
	        'utils.rendererresponse.customrenderer',
	    ),
	
	    # 设置分页（固定表达式）
	    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
	
	    # 设置分页每页显示的信息数
	    'PAGE_SIZE': 3,
	}

分页后效果：

![分页](https://i.imgur.com/E7lIKFQ.png)

>注意： ' count ' 表示总存在的信息的条数， ' next '表示下一页的地址，' pervious ' 表示上一页的地址，null表示没有上一页。可以通过这几个参数知道是否还有下一页上一页。

### 数据过滤

通常在用户进行搜索和获取数据的时候都需要对数据进行过滤，在 restful 中要实现数据的过滤其实也很简单。先创建一个数据过滤的类，将可能用到的过滤方法都放到这个类下面，然后修改settings.py文件，添加默认过滤规则，然后修改视图函数指定过滤的类。

##### 1.创建过滤的类

在utils目录下创建filters.py文件，并添加如下内容：

import django_filters
from rest_framework import filters
from stu.models import Student


	class studentfilter(filters.FilterSet):
	    # 表示以模糊搜索student表s_name中包含传入的name参数中的数据
	    name = django_filters.CharFilter('s_name', lookup_expr='icontains')
	
	    # 在s_tel字段中精确搜索传过来的tel参数的值，返回符合条件的结果
	    tel = django_filters.CharFilter('s_tel')
	
	    # 范围搜索
	    max_yuwen = django_filters.NumberFilter('s_yuwen', lookup_expr='lte')
	    min_yuwen = django_filters.NumberFilter('s_yuwen', lookup_expr='gte')
	
	    class Meta:
	        model = Student
	        fields = ['id', 's_name', 's_tel', 's_yuwen']


##### 2.修改配置文件

修改settings.py配置，设置默认过滤规则：

	REST_FRAMEWORK = {
	    # 设置过滤(固定写法)
	    'DEFAULT_FILTER_BACKENDS': ('rest_framework.filters.DjangoFilterBackend',
	                                'rest_framework.filters.SearchFilter',),
	}

>注意：在修改配置文件或者重构rest_framework的这些操作的时候，一定要注意单词拼写一定要正确，这些很多都是固定写法，如果写错了就可能导致不能运行出想要的结果。

##### 3.修改视图文件

修改视图处理类，为其制定默认过滤的类，也可以指定排序规则：

	from django.shortcuts import render
	from rest_framework import mixins, viewsets
	
	# Create your views here.
	from stu.models import Student
	from stu.serializers import studentserializer
	from utils.filters import studentfilter
	
	
	class showallstu(mixins.CreateModelMixin,
	                 mixins.DestroyModelMixin,
	                 mixins.UpdateModelMixin,
	                 mixins.RetrieveModelMixin,
	                 mixins.ListModelMixin,
	                 viewsets.GenericViewSet):
	    queryset = Student.objects.all()
	    # 定义序列化的类
	    serializer_class = studentserializer
	    # 定义过滤的类
	    filter_class = studentfilter
	
	    # 指定排序规则
	    def get_queryset(self):
	        query = self.queryset
	        return query.order_by('-id')

##### 练习：


1.查名字中包含王的学习信息：

http://127.0.0.1:8000/stu/student/?name=王

![王姓学生查询结果](https://i.imgur.com/fdUBt9n.png)

2.查找电话号码为911的学生信息：

http://127.0.0.1:8000/stu/student/?tel=911

![电话查询结果](https://i.imgur.com/55Yf634.png)


3.查找语文成绩小于60的所有学生信息：

http://127.0.0.1:8000/stu/student/?max_yuwen=60

![成绩小于60结果](https://i.imgur.com/97PUipP.png)

4.查找语文成绩在70到90之间的学生信息：

http://127.0.0.1:8000/stu/student/?min_yuwen=70&max_yuwen=90

![成绩在70-90分的学生信息](https://i.imgur.com/ROO2wBD.png)

5.查找语文成绩在70到90之间并姓王的学生信息：

http://127.0.0.1:8000/stu/student/?name=王&min_yuwen=70&max_yuwen=90

![姓王并成绩在70-90](https://i.imgur.com/AplHoCq.png)