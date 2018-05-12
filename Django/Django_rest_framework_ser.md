## Django Rest Framework 嵌套序列化关系模型

### 序列化模型与序列化关系模型

#### 序列化和反序列化

通俗的说序列化就是将一个 model 实例转换为 json 格式的过程，反序列化则是将 json 格式的数据转换为一个 model 实例的过程。

序列化模型,即对 models 里的数据模型作序列化。这个在前一篇[Django - RESTful使用指南](https://github.com/zhang15780/duo/blob/master/Django/django_restful_use1.md)中已经涉及到了，这里就不多做说明。

序列化关系模型则是对 models 里数据模型中带有关系的如 ForeignKey, ManyToManyField 和 OneToOneField 字段作序列化。

### 序列化关系模型

- StringRelatedField ： 返回一个对应关系 model 的 __unicode__() 方法的只读字符串。

- PrimaryKeyRelatedField ： 返回一个对应关系 model 的主键。

- HyperlinkedRelatedField ： 返回一个超链接，该链接指向对应关系 model 的详细数据，view-name 是必选参数，为对应的视图生成超链接。

- SlugRelatedField ： 返回一个指定对应关系 model 中的字段，需要擦参数 slug_field 中指定字段名称。

- HyperlinkedIdentityField ： 返回指定 view-name 的超链接的字段。

以上内容详细的讲解参考：[https://segmentfault.com/a/1190000010024982#articleHeader5](https://segmentfault.com/a/1190000010024982#articleHeader5)

### 嵌套序列化关系模型

ModelSerializer 类能够让你自动创建一个具有模型中相应字段的Serializer类。

这个 ModelSerializer 类和常规的 Serializer 类一样，不同的是：

- 它根据模型自动生成一组字段。
- 它自动生成序列化器的验证器，比如unique_together验证器。
- 它默认简单实现了.create()方法和.update()方法。

默认情况下，所有的模型的字段都将映射到序列化器上相应的字段。

如果你希望在模型序列化器中使用默认字段的一部分，你可以使用fields或exclude选项来执行此操作。建议你使用fields属性显示的设置要序列化的字段。这样就不太可能因为你修改了模型而无意中暴露了数据。

- fields : 

	'\_\_all\_\_' : 表明使用模型中的所有字段。

	(field_name1, field_name2....) : 表示只显示该元组中包含的字段。

	eg: 
		fields = '\_\_all\_\_'

- exclude :属性设置成一个从序列化器中排除的字段列表。
	
	eg: exclude = ('create_time', 'age')

##### 嵌套序列化

为了显示效果创建数据模型：

	class Student(models.Model):
	    s_name = models.CharField(max_length=20)
	    s_tel = models.CharField(max_length=11)
	    g_id = models.ForeignKey(Grade, related_name='gstu')
	
	    class Meta:
	        db_table = 'student'
	class Stuinfo(models.Model):
	    s_addr = models.CharField(max_length=50)
	    sid = models.OneToOneField(Student, null=True, related_name='stuinfos')
	
	    class Meta:
	        db_table = 'stuinfo'
	class Grade(models.Model):
	    g_id = models.CharField(max_length=10, unique=True)
	    g_name = models.CharField(max_length=20)
	    g_desc = models.CharField(max_length=50)
	
	    class Meta:
	        db_table = 'grade'

方式一：

使用 depth 生成嵌套关联：

serializers.py文件：

	from rest_framework import serializers
	from stu.models import Student, Stuinfo
	from grade.models import Grade
	
	class StuSerizlizer(serializers.ModelSerializer):
	    class Meta:
	        model = Student
	        depth = 1
	        fields = '\_\_all\_\_'

depth选项应该设置一个整数值，表明应该遍历的关联深度。通过depth可以将查询相关的外键关系的信息也展示出来。

结果如下图：

![depth生成嵌套序列化](https://i.imgur.com/h61lkGv.png)

方式二：

自定义字段来展示外键信息：

serializers.py文件：

	class Stuinfoserializer(serializers.ModelSerializer):
	    class Meta:
	        model = Stuinfo
	        fields = ('s_addr',)

	class StuSerizlizer(serializers.ModelSerializer):
	    stuinfos = Stuinfoserializer()
	    class Meta:
	        model = Student
	        fields = ['pk', 's_name', 's_tel', 'g_id', 'stuinfos']

>注意：```stuinfos = Stuinfoserializer()``` 和 ```fields = ['pk', 's_name', 's_tel', 'g_id', 'stuinfos'] ```中的 stuinfos 必须和定义外键时的 related_name 指定的值一致，否则就不能返回结果并且报错： ' Server Error (500) '.

这里的序列化是不可以创建和更改的，如果要反向可以创建和更改，你可以重构 ```ModeSerializer``` 的 ```create()``` 和 ```update()```方法。因为这里有一个更简单的方法去创建一个可写嵌套序列化模型，所以就不做演示了。

### 可写嵌套序列化模型

在这里有一个模块可以轻松帮我们实现创建一个可写的嵌套序列化模型： '```drf_writable_nested```

安装 drf_writable_nested：

	pip install drf_writable_nested

serializers.py文件，导入model：

	from drf_writable_nested import WritableNestedModelSerializer
	class Stuinfoserializer(serializers.ModelSerializer):
	    class Meta:
	        model = Stuinfo
	        fields = ('s_addr',)
	class StuSerizlizer(WritableNestedModelSerializer):
	    stuinfos = Stuinfoserializer()
	    class Meta:
	        model = Student
	        fields = ['pk', 's_name', 's_tel', 'g_id', 'stuinfos']

patch结果：

![修改前](https://i.imgur.com/s339DVL.png)

![修改后](https://i.imgur.com/TBPNTzL.png)

使用```WritableNestedModelSerializer```可以轻松的创建可写的嵌套序列化模型，这样将大大缩短我们开发后端API接口的时间。