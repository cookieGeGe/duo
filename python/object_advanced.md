
# 面向对象进阶

## 封装

封装：对外部世界隐藏对象的工作细节。

### 访问可见性

在前面我们写了一个Student类，在这个类中我们给对象绑定了name,school,age三个属性。在定义的时候我们使用的是双下划线开头然后跟上变量名的方式去命名的。为什么要这么做呢？在python中方法和对象的访问权限和其他的编程语言C++、Java这些事一样的，有两种访问权限既是私有的和公开的。，如果希望属性是私有的不允许外界对象访问，在给属性命名时可以用两个下划线作为开头。

Python在定义类的时候虽然能够将属性设置为私有的，但是Python却没有从语法上严格保证私有属性或方法的私密性，它只是给私有的属性和方法换了一个名字来“妨碍”对它们的访问，在知道命名规则的情况下任然能够访问到。因此大多数python程序员会遵循一种命名惯例就是让属性名以单下划线开头来隐喻属性是受保护的。

>**说明：**关于Python属性访问可见性的内容，参考致：[http://blog.csdn.net/jackfrued/article/details/79513824](http://blog.csdn.net/jackfrued/article/details/79513824)

### 包装器 / 装饰器(decorator)

装饰模式有很多经典的使用场景，例如插入日志、性能测试、事务处理等等，有了装饰器，就可以提取大量函数中与本身功能无关的类似代码，从而达到代码重用的目的。最大的好处就是可以让我们了解到函数内部到底发生了什么。前面学习了高阶函数，这里可以用高阶函数的知识写一个简单的装饰器：



```python
def record(fn):
    def wrapper(*args, **kwargs):
        print('准备执行%s函数' % fn.__name__)
        # 执行被装饰的函数，在下面这行代码的前后我们可以附加其他的代码
        # 这些代码可以让我们在执行函数时做一下额外的工作
        val = fn(*args, **kwargs)
        print('%s函数执行完成' % fn.__name__)
        print('返回了val', val)
        # 返回被装饰函数的执行结果
        return val

    return wrapper

# 通过装饰器修饰f函数，让f函数在执行过程中可以做更多额外的操作
# 把@record放到f(n)函数的定义处，相当于执行了语句：f = record(f)  因此新的同名f函数就
# 指向了wrapper，调用f函数就是调用wrapper函数（且它原来的f函数不变）它的参数就变成了wrapper的参数
@record
def f(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * f(n - 1)


def main():
    f(5)


if __name__ == '__main__':
    main()
```

    准备执行f函数
    准备执行f函数
    准备执行f函数
    准备执行f函数
    准备执行f函数
    f函数执行完成
    返回了val 1
    f函数执行完成
    返回了val 2
    f函数执行完成
    返回了val 6
    f函数执行完成
    返回了val 24
    f函数执行完成
    返回了val 120
    

通过上面的这个案例我们可以直观的了解到阶乘函数递归和回溯的过程。
在Python中有三个内置的装饰器，都是跟class相关的：staticmethod、classmethod 和property。
- staticmethod 是类静态方法，其跟成员方法的区别是没有 self 参数，并且可以在类不进行实例化的情况下调用
- classmethod 与成员方法的区别在于所接收的第一个参数不是 self （类实例的指针），而是cls（当前类的具体类型）
- property 是属性的意思，表示可以通过通过类实例直接访问的信息!



```python
class Triangle(object):
    def __init__(self, a, b, c):
        self._a = a
        self._b = b
        self._c = c

    # 类方法，同样的是发给类的消息，不是发给对象的消息
    @classmethod
    def is_valid2(cls, a, b, c):
        return a + b > c and b + c > a and a + c > b

    # 静态方法，是发送给类的消息，而不是发给对象的消息，使用方法如下
    @staticmethod
    def is_valid(a, b, c):
        return a + b > c and b + c > a and a + c > b

    @property
    def primeter(self):
        return self._a + self._b + self._c

    @property
    def area(self):
        half = self.primeter * 0.5
        return (half * (half - self._a) * (half - self._b) * (half - self._c)) ** 0.5


def main():
    a = 2
    b = 3
    c = 1
    if Triangle.is_valid(a, b, c):
        t1 = Triangle(a, b, c)
        print(t1.primeter)
        print(t1.area)
    else:
        print('不能构成三角形')

    if Triangle.is_valid2(a, b, c):
        t2 = Triangle(a, b, c)
        print(t2.primeter)
        print(t2.area)
    else:
        print('不能构成三角形')

    a = b = c = 5
    t3 = Triangle(a, b, c)
    print(t3.primeter)
    print(t3.area)
    



if __name__ == '__main__':
    main()

```

    不能构成三角形
    不能构成三角形
    15
    10.825317547305483
    

>说明:类方法和静态方法都可以在没有创建对象的情况下使用，但是累方法可以通过cls关键字访问到类的属性，而静态方法不能访问类的属性
>property属性方法通常我们使用它来返回类的某些属性信息，而修改类属性的方法则是setter。使用setter之前必须有property，通常我们将property称之为查看器，setter称为修改器。



## 继承

相信我们在定义类的时候一定遇到过两个相似的类的定义这种情况，比如定义一个猫类和一个狗类，在这里面他们都有大小，颜色，和跑，吃东西等特性，在这里面就会有很多的重复代码， Martin Fowler说过‘代码有很多中坏味道，重复是最坏的一种’所以有没有办法来解决这种相似类出现重复代码的问题的方法呢？

要解决这个问题就需要用到类的继承了。在定义类的时候我们使用的是class关键字跟上类名和一对小括号，小括号里面就是我们当前类继承的父类的名字，当然在前面我们都写的是object，因为object可以是所有类的父类。对于上面的问题，有了继承我们就只需要定义一个父类，在父类中将子类的相似的属性和方法定义好，子类去继承父类的方法即可去除掉这些重复代码了。示例如下：
```Python
class Animals(object):
    def __init__(self, name, color):
        self._name = name
        self._color = color

    def run(self):
        print('%s is running...' % self._name)

    def eat(self):
        print('%s is eating...' % self._name)

class Dog(Animals):
    def __init__(self, name, color, where):
        super().__init__(name, color) #指定name和color继承父类的初始化方法，当人默认就是继承父类,在子类有其他属性时，这一句指定继承父类的初始化方法的属性
        self._where = where
        
class Cat(Animals):
    def __init__(self, name, color, age):
        super().__init__(name, color)
        self._age = age
        
dog = Dog('jerry', 'black')
cat = Cat('Tom', 'white')
dog.run()
cat.eat()
```
上面代码的输出结果如下：
```
jerry is running...
Tom is eating...
```
在Python中类的继承同样的也支持多继承，在多继承时，如果一个方法在子类中没有定义在多个父类都有定义，在调用方法的时候则是按照从左往右的顺序调用第一个具有该方法的父类中定义的方法。当然这里是不建议使用多继承的。


### 方法重写

子类在继承父类方法之后对方法进行了重新实现，就叫做方法重写。还是用上面的例子，我们让狗跑两次只需要在Dog类里重写run方法即可：
```
class Dog(Animals):
    def __init__(self, name, color, where):
        super().__init__(name, color)
        self._where = where
        
    def run(self):
        print('%s is running...twice' % self._name)
```
上面代码的输出结果如下：
```
jerry is running...twice
Tom is eating...
```
当子类对父类中的方法重写后，给子类对象发送执行这个方法的消息的时候是执行子类重写过后的方法，不是父类的方法。

>**注意：**子类对象可以调用父类或者子类的自己的方法，父类对象不能调用子类的方法。

### 运算符重载

Python同样支持运算符重载，我们可以对类的专有方法进行重载，实例如下：
```Python
from math import gcd


class Fraction(object):

    def __init__(self, num, den):
        if den == 0:
            raise ValueError('分母不能为0') # raise表示引发一个值错误
        self._num = num
        self._den = den

    def __str__(self):
        if self._num == 0:
            return '0'
        elif self._den == 1:
            return self._num
        else:
            return '%d/%d' % (self._num, self._den)

    @property
    def den(self):
        return self._den

    @property
    def num(self):
        return self._num

    def add(self, other):
        return Fraction(self._num * other.den + \
                        other.num * self._den, self._den * other.den)

    # 运算符重写：
    # 即是在做运算的时候可以直接使用符号即可获得相应的结果
    def __add__(self, other):
        return self.add(other)

    def sub(self, other):
        return Fraction(self._num * other.den - \
                        other.num * self._den, self._den * other.den)

    def __sub__(self, other):
        return self.sub(other)

f1 = Fraction(-3,5)
f2 = Fraction(1,2)
print(f1.add(f2))
print(f1.sub(f2))
print(f1 + f2)
print(f1 - f2)
```
程序执行结果如下：
```
-1/10
-11/10
-1/10
-11/10
```

## 多态

### 抽象类

抽象类是包含抽象方法的类，而抽象方法不包含任何可实现的代码，只能在其子类中实现抽象函数的代码。Python并没有从语言层面支持抽象类概念，可以通过abc模块来制造抽象类的效果。因此在Python中要实现抽象类之前需要导入abc模块中的元类(ABCMeta)和装饰器(abstractmethod)。
- 元类（ABCMeta）

在创建抽象类时使用metaclass=ABCMeta表示将该类创建为抽象类，metaclass=ABCMeta指定该类的元类是ABCmeta。所谓元类就是创建类的类。

- 装饰器(abstractmethod)

在定义方法的时候使用这个装饰器包装，将制定的方法定义为抽象方法。

因为抽象方法不包含任何可实现的代码，因此其函数体通常使用pass。下面演示抽象类的实现和多态。所谓的多态指的是在抽象类中定义一个方法，可以在其子类中重新实现，不同子类中实现的方法也不尽相同。



```python
"""
某公司有三种类型的员工 分别是部门经理、程序员和销售员
需要设计一个工资结算系统 根据提供的员工信息来计算月薪
部门经理的月薪是每月固定15000元
程序员的月薪按本月工作时间计算 每小时150元
销售员的月薪是1200元的底薪加上销售额5%的提成
"""
from abc import ABCMeta, abstractmethod

class Employee(object, metaclass=ABCMeta):

    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @abstractmethod  # 将该方法定义为抽象方法，强制子类实现get_salary方法
    def get_salary(self):
        pass


class Manager(Employee):

    def get_salary(self):
        return 15000


class Programmer(Employee):

    def __init__(self, name):
        super().__init__(name)
        self._working_hour = 0

    @property
    def working_hour(self):
        return self._working_hour

    @working_hour.setter
    def working_hour(self, hour):
        self._working_hour = hour if hour > 0 else 0

    def get_salary(self):
        return 150.0 * self._working_hour

class Salesman(Employee):

    def __init__(self, name, sales=0):
        super().__init__(name)
        self._sales = sales

    @property
    def sale(self):
        return self._sales

    @sale.setter
    def sale(self, sale):
        self._sales = sale if sale > 0 else 0

    def get_salary(self):
        return self._sales * 0.05 + 1200.0


def main():
    emps = [Manager('令狐冲'), Programmer('东方不败'),
            Manager('岳不群'), Salesman('林平之'),
            Salesman('风清扬'), Programmer('仪琳师妹'),
            ]

    for emp in emps:
        if isinstance(emp, Programmer):  # 识别对象的类型，判断给定的对象是否是后面给的类型
            emp.working_hour = int(input('请输入%s本月工作时间：' % emp.name))
        if isinstance(emp, Salesman): 
            emp.sale = int(input('请输入%s本月销售金额：' % emp.name))
        # 同样是调用get_salary()方法，但是不同的员工做出了不同行为
        # 因为三个子类都重写了get_salary()方法，所以此处有多态行为
        print('%s本月工资为：￥%.2f元' % (emp.name, emp.get_salary()))


if __name__ == '__main__':
    main()

```

    令狐冲本月工资为：￥15000.00元
    请输入东方不败本月工作时间：25
    东方不败本月工资为：￥3750.00元
    岳不群本月工资为：￥15000.00元
    请输入林平之本月销售金额：15000
    林平之本月工资为：￥1950.00元
    请输入风清扬本月销售金额：5
    风清扬本月工资为：￥1200.25元
    请输入仪琳师妹本月工作时间：56
    仪琳师妹本月工资为：￥8400.00元
    

>**注意:**

>1.抽象类不能创建对象（即是不能实例化）,抽象类存在的意义是专门拿给其他类继承的。

>2.类在没有抽象方法的时候不是抽象类可以实例化，有抽象方法的时候就是抽象类不能进行实例化

>3.当子类覆盖了父类的抽象方法的之后，子类就不是抽象类了，就可以实例化,当子类没有覆盖父类的抽象方法，从父类继承的时候子类也是抽象类，不能实例化

>4.抽象方法 必须在子类进行重写，否则就会报错
