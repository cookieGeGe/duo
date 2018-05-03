

## Django 中间件

Django1.9版本以后，我们从浏览器发出一个请求 Request，得到一个响应后的内容 HttpResponse ，这个请求传递到 Django的过程如下：

![Django网页请求处理流程](https://i.imgur.com/HIDNoad.png)

也就是说，每一个请求都是先通过中间件中的 process_request 函数，这个函数返回 None 或者 HttpResponse 对象，如果返回前者，继续处理其它中间件，如果返回一个 HttpResponse，就处理中止，返回到网页上。

###中间件（类）的几种方法

中间件可以定义的几种方法，分别是：

- process_request(self,request) ： 在处理url请求之前执行

- process_view(self, request, callback, callback_args, 
callback_kwargs) ： 调用视图之前执行

- process_template_response(self,request,response) ： 只有当views函数中返回的对象中具有render方法，才会直接调用

- process_response(self, request, response) ： 在响应返回浏览器之前调用

### 自定义中间件

在Django中我们可以自己写一个继承了MiddlewareMixin的类，来实现自定义中间件。通过```from django.urls.deprecation import MiddlewareMixin```导入MiddlewareMixin。

为中间件创建一个目录Middle，并在Middle目录下创建middle1.py:

```
from django.utils.deprecation import MiddlewareMixin


class middle11(MiddlewareMixin):
    def process_request(self, request):
        print("中间件1请求")

    def process_response(self, request, response):
        print("中间件1返回")
        return response


class middle2(MiddlewareMixin):
    def process_request(self, request):
        print("中间件2请求")

    def process_response(self, request, response):
        print("中间件2返回")
        return response


class middle3(MiddlewareMixin):
    def process_request(self, request):
        print("中间件3请求")

    def process_response(self, request, response):
        print("中间件3返回")
        return response
```

在项目目录下的settings.py文件的MIDDLEWARE中添加如下三行：

```
	'Middle.middle1.middle11',
    'Middle.middle1.middle2',
    'Middle.middle1.middle3',
```

当我们在浏览器中访问一个页面的时候在控制台就会看到如下的结果：

![middleware自定义演示结果](https://i.imgur.com/OXBnev1.png)

从这里也向我们证实了当一个请求进来的时候，会通过所有的中间件处理，并且当请求获得相应时也会通过中间件去处理。

### 中间件应用场景

由于中间件工作在 视图函数执行前、执行后适合所有的请求/一部分请求做批量处理。

- 1、做IP限制

放在 中间件类的列表中，阻止某些IP访问了；

- 2.URL访问过滤

如果用户访问的是login视图（放过）

如果访问其他视图（需要检测是不是有session已经有了放行，没有返回login），这样就省得在 多个视图函数上写装饰器了！

- 3、缓存

客户端请求来了，中间件去缓存看看有没有数据，有直接返回给用户，没有再去逻辑层 执行视图函数

### 练习：

1.利用用中间件，实现让所有页面都必须在进行用户登录后才能访问。

2.中间件统计，某个网页的访问次数。


