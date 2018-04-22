
## 字符串

字符串是 Python 中最常用的数据类型。我们可以使用引号('或")来创建字符串。Python 不支持单字符类型，单字符也在Python也是作为一个字符串使用。

### 字符串索引（下标）

所谓“下标”，就是编号，就好比超市中的存储柜的编号，通过这个编号就能找到相应的存储空间!

python的字串列表有2种取值顺序:

从左到右索引默认0开始的，最大范围是字符串长度少1
从右到左索引默认-1开始的，最大范围是字符串开头

如果你要实现从字符串中获取一段子字符串的话，可以使用变量 [头下标:尾下标]，就可以截取相应的字符串，其中下标是从 0 开始算起，可以是正数或负数，下标可以为空表示取到头或尾。
例如：
```Python
str1 = 'hello world!'

print(str1[:5]

```
其结果为'hello'，这就是通过下标将字符串切片的方法。

> **注意：**字符串使用下标切片的时候入上面例子中是从str1这个字符串中取出0到5的字符，但是不包括下标为5的字符，因此在使用字符串进行切片的时候应注意切片的是一个半闭半开区间。


### 字符串常用方法

字符串的方法有很多详细的参考[Python3 字符串](http://www.runoob.com/python3/python3-string.html)

通过以下代码展示部分方法的使用：

```Python
str1 = 'hello, world!'
print(len(str1))  
#返回字符串的长度
print(str1.capitalize())  
#将字符串的第一个字符转换为大写!
print(str1.upper())   
#将字符串中所有小写字母转换为大写
print(str1.lower())   
#将字符串中所有字母转化为小写
print(str1.title())   
#将字符串中所有的单词首字母都大写
print(max(str1))     
#返回字符串中最大的字母（以ASCII码表为参考）同理也有min()方法
print(str1.find('or'))   
#从左到右进行查找，找到了返回索引，找不到返回-1，从右往左查找使用rfind()方法
#print(str1.index("shit"))   #z找不到就出现错误，找到了返回索引
print(str1.startswith("he"))  
#检查一个字符串是不是以he开头的，同理也有endswith()方法，检测以哪个字符串结尾
print(str1.center(50,'*'))  
#指定长度为50，不够用*填充，str1居中对齐
str2 = ' asds   ldjf  '
print(str2.strip())     
#跳过字符串两端的空格，当然也可以指定跳过的字符，默认为跳过空格
print(str2.split('d'))    
#以字符串d对当前字符串进行切割，切割后得到一个列表
print(str1[-1:-3:-1])     
#表示反向从-1取到-3,后面的-1表示反向选取步长为1，,类似range的用法,只有用负索引时才能给得到反向
print((str1[::-1]))    
#实现字符串的反转


```

## 列表

List（列表） 是 Python 中使用最频繁的数据类型。

列表可以完成大多数集合类的数据结构实现。它支持字符，数字，字符串甚至可以包含列表等（即嵌套）。

列表与字符串类似同样的支持下标索引的方式取出列表中的元素，绝大多数字符串支持的运算方法在列表中也同样适用。

### 列表中元素的增删改和遍历

- 列表中增加元素
> 当需要生成一个有特定规律的列表时，可以使用列表生成式快速的生成一个列表：



```python
list1 = [x * x for x in range(10)]

print('list1=',list1)
```

    list1= [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    

- 列表中元素更新

列表中的元素的更新可以直接使用索引对列表指定位置进行修改：



```python
list2 = ['hello', 'world', 2562, 1589]
print('更新前的list：',list2)

list2[1] = 'Python'
print('更新后的list：',list2)
```

    更新前的list： ['hello', 'world', 2562, 1589]
    更新后的list： ['hello', 'Python', 2562, 1589]
    

- 列表中元素的删除
使用 del 语句来删除列表的的元素，如下实例：


```python
list3 = ['hello', 'world', 1997, 2000]
print ('删除前的列表', list3)

del list3[2]
print ("删除第三个元素后的列表 : ", list3)
```

    删除前的列表 ['hello', 'world', 1997, 2000]
    删除第三个元素后的列表 :  ['hello', 'world', 2000]
    

- 列表中元素的遍历

通过循环对列表的遍历方式有很多中，但是通常我们使用如下的方法，既能得到值也能得到索引：



```python
list4 = ['hello', 'world', 'Python', 1997, 2000]
for index,value in enumerate(list4):
    print(index, ':', value)
```

    0 : hello
    1 : world
    2 : Python
    3 : 1997
    4 : 2000
    

### 列表中的常用方法和函数：

我们通过如下的代码来了解列表的函数和方法的使用：
```Python
f = [100, 200, 500]
# enumerate()枚举的方式可以即返回list中的值，同时也可以返回其对于的索引值
for index, val in enumerate(f):
    print(index, val)
f.append(123)   #向列表最后追加元素123
f.insert(1, 300)  #在列表索引为1的元素之后插入300
f.insert(-1, 121)  # 在索引为-1的元素之前插入121
f.insert(100, 10)  # 如果不存在索引100则在最后添加
f.remove(200)  # 如果200不存在会报错，存在就删除，可以使用判断是否存在，然后在删除，eg：f.remove(5999) if 5999 in f
f.pop()  # 默认是移除最后一个，可以移除指定索引的元素f.pop(i)
f.index(500, 2, 5)  # 查找500在索引为2到5之间查找，如果找不到会报错，找到了返回索引
# f.clear() #表示清空
f.reverse()  # 表示将f这个列表反向，直接修改f
print(reversed(f))
f.sort()  # 表示将f进行排序，默认升序排列，可以使用reverse反向排序，也可以指定排序关键字key，直接修改f
sorted(f, reverse=True, key=len)  # python内置的排序函数，返回值为一个排好序的新list,
# key=len，key表示指定排序的方式
# 默认是升序排列，如果需要降序，可以通过reverse=True来实现
# python 中的函数几乎都是没有副作用的函数（调用函数后不会影响传入的参数）
print(reversed(f))  # 类似sorted()函数,但是它返回的值不是list列表
print(sorted(f))

print(f)

```



## 元组

元组是另一个数据类型，元组类似于列表，列表的切片方法在元组中依然适用，但是元组和列表也有两个不同的地方：
> 创建元组使用的是(),而创建列表使用的是[]
> 列表中的元素可以进行增删改，元组是不支持增删改的，只能对整个元组进行删除

如下示例简单介绍如何创建元组和元组转换为列表：
```Python
list1 = ['hello', 'python']
tuple1 = (1, 2, 3, 4)  # 创建元组
print(tuple1)
tuple2 = tuple(list1)  # 将列表转换为元组
list2 = list(tuple1)   #将元组转换为列表

```


## 集合

在Python中set是基本数据类型的一种集合类型，它有可变集合(set())和不可变集合(frozenset)两种。创建集合set、集合set添加、集合删除、交集、并集、差集的操作都是非常实用的方法。Python中的集合跟数学上的集合是一致的，内部的元素是无序不重复的。

- 创建集合
集合的创建很简单，使用花括号{}，在括号中指定参数，多个参数用,隔开即可，在集合中set括号中需要的参数的数据类型有：序列（包括字符串、列表、元组），字典或者是另一个集合，但是不能是数值类型，如int类型。



```python
list1 = [8, 9, 7, 6, 8, 9, 7]
set1 = set(list1)
set2 = {1, 1, 2, 2, 3, 3}


print(set1)
print(set2)
```

    {8, 9, 6, 7}
    {1, 2, 3}
    

- 集合添加元素
python 集合的添加有两种常用方法，分别是add和update。


```python
set2 = {1, 1, 2, 2, 3, 3}

set2.add('5')
set2.update('Python')

print(set2)
```

    {1, 2, 3, 't', '5', 'n', 'y', 'h', 'P', 'o'}
    

- 集合删除元素
集合删除操作方法：remove


```python
set3 = {1, 2, 3, 't', '5', 'n', 'y', 'h', 'P', 'o'}
set3.remove('5')

print(set3)
```

    {1, 2, 3, 't', 'n', 'y', 'h', 'P', 'o'}
    

### 集合的交集并集

通过如下代码了解集合的计算交集和并集的用法：

```Python
set1 = {1, 1, 2, 2, 3, 3}
set2 = {1, 3, 5, 7, 9}
set3 = set2 & set1   # 计算交集
set3 = set1.intersection(set2)  # 计算交集
print(set3)
set3 = set2 | set1   # 计算并集
set3 = set1.union(set2)  # 计算并集
print(set3)
set3 = set1.difference(set2)  # 计算差集
print(set3)
set3 = set2 ^ set1     # 计算对称差
set3 = set1.symmetric_difference(set2)  # 计算对称差
print(set3)
print(set1.issubset(set2))  # 判断set2是否是set1的子集
print(set1.issuperset(set2))  # 判断set2是否是set1的父集
set1.discard(100)    #如果集合中存在100，则删除，不存在不报错
print(set1)
```

## 字典

字典是另一种可变容器模型，且可存储任意类型对象。
字典的每个键值(key=>value)对用冒号(:)分割，每个对之间用逗号(,)分割，整个字典包括在花括号({})中 ,格式如下所示：
d = {key1 : value1, key2 : value2 }
键必须是唯一的，但值则不必。
值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。!

### 访问字典中的值

因为字典中键的唯一性，所以访问字典中的值的时候，只需要把相应的键放入方括弧中即可访问到键对应的值，如下实例:


```python
dict1 = {'name': '王大锤', 'age': 38, 'gender': 'True'}
print(dict1['name'])
```

    王大锤
    

### 修改字典

向字典添加新内容的方法是增加新的键/值对，修改已有键/值对的方法类似于修改列表中的元素，如下实例:



```python
dict1 = {'name': '王大锤', 'age': 38, 'gender': 'True'}
print(dict1)
dict1['school'] = '成都七中' 
dict1['age'] = 18
print(dict1)
```

    {'name': '王大锤', 'age': 38, 'gender': 'True'}
    {'name': '王大锤', 'age': 18, 'gender': 'True', 'school': '成都七中'}
    

### 删除字典元素

删除字典中的元素有几种，能删单一的元素也能清空字典，清空只需一项操作。



```python
dict2 = {'Name': '王大锤', 'Age': 7, 'Class': 'First'}

del dict2['Name'] # 删除键 'Name'
print (dict2)
#dict2.clear()     # 清空字典
del dict2         # 删除字典


```

    {'Age': 7, 'Class': 'First'}
    

> **注意：**当使用del删除字典、或者字典中的元素或者其他的列表的时候如果删除的内容不存在，都会报错。在删除之前都应该先判断是否存在

### 列表中的常用的方法

Python字典包含了以下内置方法：
![字典常用方法](http://upload-images.jianshu.io/upload_images/10930505-c51b1ffae2a48aa6..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 练习1：文字跑马灯


```python
import os
import time


def main():
    str1 = "客上天然居居然天上客,人过大佛寺寺佛大过人。"
    while True:
        os.system('cls')
        str1 = str1[1:] + str1[0]
        print(str1)
        time.sleep(0.5)


if __name__ == '__main__':
    main()
```

## 练习2：打印杨辉三角


```python
# def striangle():


def main():
    lines = int(input('请输入杨辉三角的行数：'))
    total_triangle = [[0 for x in range(i)] for i in range(1, lines + 1)]
    for i in range(1, lines + 1):
        for x in range(i):
            if x == 0 or x == (i - 1):
                total_triangle[i - 1][x] = 1
                print(total_triangle[i-1][x],end=' ')
                x == i-1 and print()
            else:
                total_triangle[i - 1][x] = total_triangle[i - 2][x - 1] + total_triangle[i - 2][x]
                print(total_triangle[i-1][x], end=' ')


if __name__ == '__main__':
    main()

```

    请输入杨辉三角的行数：7
    1 
    1 1 
    1 2 1 
    1 3 3 1 
    1 4 6 4 1 
    1 5 10 10 5 1 
    1 6 15 20 15 6 1 
    

## 练习3：约瑟夫环


```python
"""

有30个人在海上遇险，为了能让一部分人活下来不得不将其中15个人扔到海里面去，
有个人想了个办法就是大家围成一个圈，由某个人开始从1报数，报到9的人就扔到海里面，
他后面的人接着从1开始报数，报到9的人继续扔到海里面，直到扔掉15个人。
由于上帝的保佑，15个基督徒都幸免于难，问这些人最开始是怎么站的，哪些位置是基督徒哪些位置是非基督徒。

"""

def main():
    ring = [1] * 30
    ring_index = [i for i in range(30)]
    print('死掉的人的位置是：')
    while ring.count(0) < 15:
        for i in range(8):
            ring_index.append(ring_index[0])
            ring_index.remove(ring_index[0])
        kill = ring_index[0]
        print(ring_index[0] + 1, end=' ')
        ring_index.remove(ring_index[0])
        ring[kill] = 0
    print()
    for person in ring:
        print('基' if person else '非', end='')
if __name__ == '__main__':
    main()
```

    死掉的人的位置是：
    9 18 27 6 16 26 7 19 30 12 24 8 22 5 23 
    基基基基非非非非非基基非基基基非基非非基基非非非基非非基基非

## 总结

字符串、列表、元组、集合、字典这些东西有很多的相似之处也有很多不同之处，比如字符串和列表，元组都支持索引，字符串是和元组不能更改里面的内容，而列表却可以修改里面的内容。知识点很多，但是找到相似之处和不同之处掌握这些知识点还是很容易的，但是要想熟练的使用这些就还需要多看多练才可以了。
