## Django 分页

要使用Django实现分页器,需要从Django中导入Paginator模块

```
from django.core.paginator import Paginator
```

### 1.创建Paginator对象：

```
page1 = Paginator(list1, num)
```

>注意：list1为需要分页显示的对象列表，num为每页显示的记录的数量。

### 2.分页对象有的方法：

- count : 获取总的记录数，即需要分页显示的对象列表的长度;

- num_pages : 打印总的页数,即总记录数除以每页显示的条目数;

- page_range : 获得页数的列表；

- page(num) : 得到一个第num页的page对象。

### 3.page对象的方法：

- paginator : 转换为一个paginator对象；

- object_list : 获取当前page对象中的所有记录；

- has_next : 判断当前页是否有下一页；

- next_page_number : 获取下一页的页码；

- has_previous : 判断当前要是否有上一页；

- previous_page_number : 获取上一页的页码;

- has_other_pages : 判断收有其他页;

- start_index : 获取当前页第一条记录的序号；

- end_index : 获取当前页的最后一条记录的序号。

### 练习：

在[Django上传文件](./django_pic.md)显示图片的基础上修改为显示所有的图片和描述，但是显示是分页显示，每页显示3条记录。

1.修改showimg视图函数：

```
from django.core.paginator import Paginator

def showimg(request):
    pages = request.GET.get('page', 1)
    lastimg = ImgUpload.objects.all()
    paginator = Paginator(lastimg, 3)

    pag = paginator.page(pages)

    return render(request, 'showimg.html', {'lastimg': pag})
```

2.修改显示showimg.html静态页面为：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>显示添加的图片</title>
</head>
<body>
{% for img in lastimg %}
    <img width="300" height="300" src='/media/{{ img.i_img }}'>
    <p>{{ img.i_desc }}</p>
<hr />
{% endfor %}

<p>一共有{{ lastimg.paginator.num_pages }}页/一共有{{ lastimg.paginator.count }}条数据</p>
{% if lastimg.has_previous %}
    <a href="/img/showimg/?page={{ lastimg.previous_page_number }}">上一页</a>
{% endif %}

{#当前是第{{ lastimg.number }}页#}

{% for num in lastimg.paginator.page_range %}

    {% if num == lastimg.number %}
        {{ num }}
    {% else %}
        <a href="/img/showimg/?page={{ num }}"> {{ num }} </a>

    {% endif %}
{% endfor %}


{% if lastimg.has_next %}
    <a href="/img/showimg/?page={{ lastimg.next_page_number }}">下一页</a>
{% endif %}
</body>
</html>
```

测试结果为：

![分页结果](https://i.imgur.com/CA4jhaD.png)

