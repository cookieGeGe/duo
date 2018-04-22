


## 关键字参数(**kwargs)




```python
def foo(**kw):
    if 'y' in kw:
        print(kw['y'])
foo(x=123,y='1232')
```

    1232
    

如上面的示例关键字参数用于函数调用，通过“键-值”形式加以指定。这种方式可以根据传入的参数来决定函数的运行方向。可以让函数更加清晰、容易使用，同时也清除了参数的顺序需求，及时传入了多个与函数无关的参数同样也不影响函数的运行，只有和函数相关的参数才会被使用，不相关的不影响函数执行。

示例：


```python
def say_hello(**kwargs):
    if 'name' in kwargs:
        print('您好,%s' % kwargs['name'])
    if 'age' in kwargs:
        age = kwargs['age']
        if age <= 16:
            print('你还是一个小屁孩！')
        else:
            print('你是一个成年人')
    if 'name' not in kwargs and 'age' not in kwargs:
        print('请提供个人信息')

dict1 = {'name': '王大锤', 'age': 15, 'tel': '12345678'}

say_hello(**dict1)
```

    您好,王大锤
    你还是一个小屁孩！
    

> **说明：**在向函数传入可变参数的时候，这些可变参数在函数调用时自动组装为一个tuple，而在向函数传入关键字参数的时候这些关键字参数在函数内部自动组装为一个dict。所以上面的例子中向函数传入的便是一个字典，程序同样能够正常执行。

## 命名关键字参数

写了这么久的程序，相信大家一定遇到过这种情况，就是在使用某个函数的时候，给函数指定参数的时候必须得写成"agrs=..."的形式才能将参数传进去，而有些参数不需要这么写，比如下面这个例子：
```Python
def person(name, age, *, city, job):
    print(name, age, city, job)

x = person('zhang',18,city='chengdu',job=None)
```
如上面的例子，当我们需要限制用户输入参数名的时候我们就需要用到命名关键字参数了，上面的例子就表示只接受参数名为city和job的关键字参数。定义命名关键字参数和关键字参数（**kw）不同，命名关键字参数的定义需要一个特殊分隔符*，*之后的所有参数被视为命名关键字参数，在传参时必须跟上参数名，如果没有参数名，调试的时候就会报错。命名关键字参数也支持默认参数。
>**注意：**定义不定长参数的时候使用的是*args，定义关键字参数的时候使用的是**kwargs，而定义命名关键字参数的时候是在定义函数的时候在参数中插入分隔符*,*之后的参数被视作是关键字参数。

## 高阶函数(Higher-order function)

高阶函数，听名字都觉得好高大上，好难懂的样子。那么事实是不是这样呢？对于我们这种初学者小白来说确实是挺难的，但是知道它的原理后其实也不是太难理解，具体怎么回事呢？且看我一步一步的分析：

以Python内置的求绝对值的函数为例：



```python
print(abs(-10))
print(abs)
print(id(abs))
```

    10
    <built-in function abs>
    2067312
    

由此可见abs(-10）是函数调用，而abs是函数本身，它的id是2067312，如果某个变量的id和abs的id一样会怎么样呢？


```python
foo = abs

print(foo)
print(abs)

print('foo的id：', id(foo))
print('abs的id：', id(abs))

print(foo(-10))
print(abs(-10))
```

    <built-in function abs>
    <built-in function abs>
    foo的id： 2067312
    abs的id： 2067312
    10
    10
    

调用函数abs()函数和调用变量foo()得到结果是相同的，而且foo和abs的id是相同的。既是说明变量foo已经指向了abs函数本身，也说明了函数本身是可以赋值给变量的，同时这里的示例再次向我们证实了Python中变量赋值是地址赋值。

既然abs可以赋值给foo，那么abs能不能被其他变量赋值呢？答案是肯定的，因为abs也是一个变量，只是它内置的存储了求绝对值函数的地址。不信可以通过以下代码测试：
```Python
a = 10
print(abs(-5))
print(id(abs))
print(id(a))
abs = a
print(id(abs))
print(id(a))
print(abs(-5))

```
如上代码，将a的值赋值给abs后abs存储的就是a的地址，后两个输出的id是同一个id，而且最后一行还会报错，因为在最后这里abs存的是a的值，就调不到绝对值函数了，因此在最后就会报错。通过这里也就说明了我们完全可以把函数名看作是一个变量，只是这个变量比较特殊，在一开始就指向了一个函数可以调用。

>**注意：**在实际代码中我们绝对不能像上面这么写，这里是为了说明函数名也是变量。恢复abs函数，重启Python交互环境即可。由于abs函数实际上是定义在import builtins模块中的，所以要让修改abs变量的指向在其它模块也生效，要用import builtins; builtins.abs = 10。

既然变量可以指向函数，函数的参数也能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

这里就以计算一个列表中所有元素之和和所有元素之积做一个示例：


```python
def calc(my_list, op):
    total = my_list[0]
    for i in my_list[1:]:
        total = op(total, i)
    return total


def add(x, y):
    return x + y


def mul(x, y):
    return x * y


list1 = [1, 2, 3, 5, 8, 9, 10, 15, 18, 19, 26]
sum1 = calc(list1, add)
mul1 = calc(list1, mul)
print('list1中所有项的和为：', sum1)
print('list1中所有项的乘积为：', mul1)
```

    list1中所有项的和为： 116
    list1中所有项的乘积为： 2881008000
    
