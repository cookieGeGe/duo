
# 面向对象

## 概述

• 面向过程：根据业务逻辑从上到下写垒代码

• 函数式：将功能代码封装到函数中，无需重复编写整个功能代码的实现，仅调用即可

• 面向对象：对函数进行封装，这样能够更快速的开发程序，减少了重复代码的重写过程!

Python从设计之初就已经是一门面向对象的语言!因此毋庸置疑python用于面向对象的开发。在面向对象的开发中的核心思想就是一切皆对象，在面向对象编程中两个比较重要的概念就是类和对象。对象是面向对象编程的核心，在使用对象的过程中，为了将具有共同特征和行为的一组对象抽象定义，提出了另外一个新的概念——类!

举个简单的例子就是：我们现实生活中生产一辆汽车都是按照设计图纸来生产制造的，类就相当于设计图纸，按照图纸制造出来的汽车就是对象，因此关于类和对象之间的关系就是，类是对象的蓝图或者模板，而对象是类的实例。

## 类和对象

常言道：人以类聚，物以群分。这是现实生活中人们常说的一句话，其实在程序开发中也不过如此，类是具有相似内部状态和运动规律的实体的集合或者是具有相同属性和行为事物的统称!类是抽象的,在使用的时候通常会找到这个类的一个具体的存在,使用这个具体的存在。这个具体存在就是对象。在现实生活中定义一个类是很简单的，那么在程序中如何定义一个类呢？

在程序中定义一个类也很简单，只需要做两件事，一是数据抽象，抽取对象共同的静态特征定义对象的属性，另一个是行为抽象，抽象对象共同的动态特征既是为对象定义方法。具体方法如下：
```Python
class 类名(父类):
    类方法....

```
>**说明：**类名：大写字母开头，遵循驼峰命名法，尽量做到见名知意。父类：表示这个类是从哪个类取继承的，默认为object。类方法：包括类的初始化方法和其他的方法（定义在类里面的函数）。

接下来我们通过定义一个拥有学习和看视频方法的学生类来了解类是如何定义的：

```Python
class Student(object):
    
    # __init__方法为类的初始化方法，用于在创建对象时对对象进行初始化操作
    # 这里为学生对象绑定了姓名、学校、年龄三个属性
    def __init__(self, name='', school='', age=8):
        self.__name = name
        self.__course = course
        self.__age = age

# 创建一个Student类的实例对象student1，他的名字是王大锤，18岁，学校是北大青鸟
student1 = Student('王大锤', '北大青鸟', 22)
```

### self参数

相信大家不难发现，在上面定义Student类的代码中，每一个方法（包括初始化方法）的第一个参数都是self，那么这个参数self是干什么的呢？

其实这里的self代表的是实例本身，而不是类：


```python
class Foo(object):
    def __init__(self):
        print(self)
        print(id(self))
        
test = Foo() # 得到实例化对象test

print(test)
print(id(test))

        
```

    <__main__.Foo object at 0x04294AF0>
    69815024
    <__main__.Foo object at 0x04294AF0>
    69815024
    

通过以上的示例不难发现，打印的self和我们直接打印对象的id是一样的。这就说明了这两个是指向同一个地址的也就是说self实际上就代表的是实例化后的对象本身。

### 类的方法

类的方法，它是类的行为抽象。就如前面提到的一样，简单的说类就是定义在类定义里面的函数，它的第一个参数必须是self。我们可以通过调用方法给对象发消息，去抽象实现对象的动态特征。如抽象“王大锤学习和看视频的行为”：



```python
class Student(object):

    def __init__(self, name='', school='', age=8):
        self.__name = name
        self.__school = school
        self.__age = age

    def study(self, course):
        print('%s正着学习%s' % (self.__name, course))

    def watch_av(self):
        if self.__age >= 18:
            print('%s正在观看视频....' % self.__name)
        else:
            print('%s，我们推荐你观看天线宝宝..' % self.__name)


student1 = Student('王大锤', '北大青鸟', 22)

student1.study('数学')
student1.watch_av()
```

    王大锤正着学习数学
    王大锤正在观看视频....
    


```python
练习（一）：实现一个倒计时时钟：
```


```python
import time


class Countdown:
    """
    倒计时
    """
    def __init__(self, hour=0, minute=0, second=0):
        self._hour = hour
        self._minute = minute
        self._second = second
        self._seconds = self._hour * 60 * 60 + self._minute * 60 + self._second

    def down(self):
        self._seconds -= 1
        self._second = self._seconds % 60
        self._minute = self._seconds // 60 % 60
        self._hour = (self._seconds // 60 // 60) % 24

    def show_time(self):
        return self._hour, self._minute, self._second


def main():
    hour, minute, second = eval(input('请输入倒计时的多长时间(没有就输0):'))
    countdown1 = Countdown(hour, minute, second)
    print('剩余时间为：%02d:%02d:%02d' % countdown1.show_time())
    while True:
        time.sleep(1)
        countdown1.down()
        print('剩余时间为：%02d:%02d:%02d' % countdown1.show_time())
        if countdown1.show_time()[2] == 0 and countdown1.show_time()[0] != 0 \
                and countdown1.show_time()[1] != 0:
            break


if __name__ == '__main__':
    main()
```

