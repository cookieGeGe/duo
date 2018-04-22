# 深复制、浅复制

## 定义

- 在Python中对象的赋值其实就是对象的引用。当创建一个对象，把它赋值给另一个变量的时候，python并没有复制这个对象，只是将变量的地址指向了这个对象。（python是地址赋值）

>**浅复制**：复制了最外围的对象本身，内部的元素都只是复制了一个引用而已。通俗的理解也就是将某个变量指向了该对象的地址，复制前后的两个变量指向同一个地址。

>**深复制**：外围和内部元素都进行了复制，而不是引用。也就是，把对象复制存储到另一个地址空间上，并使变量指向这个新的地址空间。

## 应用范围

>1，切片可应用于：列表、元组、字符串，但不能应用于字典。 
>2，深浅拷贝，既可应用序列（列表、元组、字符串），也可应用字典。

## 图解深、浅复制：

- 浅复制：

>假设存在一个列表[1, 2, 3, 4, 5, 6]它的地址为“40005000”，其浅复制过程如图：
![浅复制](http://upload-images.jianshu.io/upload_images/10930505-010b6bc01765993a..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 深复制：

>同样假设存在一个列表['A', 'B', 'C']它的地址为“60008000”，其深复制过程如图：
![深复制](http://upload-images.jianshu.io/upload_images/10930505-fe0c18bdbd428ca5..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 程序示例：

- 浅复制


```python
list1 = [1, 2, 3, 4, 5]

print('list1:', list1)
print('list1的地址为：', id(list1))
print()

list2 = list1    #进行列表浅复制


print('list2', list2)
print('list2的地址为：', id(list2))
print()

list1[2] = 10
print('list1:', list1)
print('list2:', list2)
print()

list2[3] = 20
print('list1:', list1)
print('list2:', list2)
```

    list1: [1, 2, 3, 4, 5]
    list1的地址为： 1614951040008
    
    list2 [1, 2, 3, 4, 5]
    list2的地址为： 1614951040008
    
    list1: [1, 2, 10, 4, 5]
    list2: [1, 2, 10, 4, 5]
    
    list1: [1, 2, 10, 20, 5]
    list2: [1, 2, 10, 20, 5]
    

- 深复制


```python
list1 = ['A', 'B', 'C', 'D', 'E']

print(list1)
print('list1的地址为：', id(list1))

print()
list2 = list1[:]    #进行列表深复制
print()

print(list2)
print('list2的地址为：', id(list2))

print()
list1[2] = 'Hello'
print('list1:', list1)
print('list2:', list2)

print()
list2[4] = 'World'
print('list1:', list1)
print('list2:', list2)
```

    ['A', 'B', 'C', 'D', 'E']
    list1的地址为： 1614950970632
    
    
    ['A', 'B', 'C', 'D', 'E']
    list2的地址为： 1614950980936
    
    list1: ['A', 'B', 'Hello', 'D', 'E']
    list2: ['A', 'B', 'C', 'D', 'E']
    
    list1: ['A', 'B', 'Hello', 'D', 'E']
    list2: ['A', 'B', 'C', 'D', 'World']
    

## 总结：

- 浅复制：地址复制，复制前后两对象指向相同的地址，指向同一段空间，修改任意一个对象，另一对象也同样的被修改。

- 深复制：将原来的内容复制到一段新的内存空间，复制前后的两对象指向不同的地址。修改任意对象中的值，只影改变自己。

>**注意:**以后在做数据的清洗、修改或者入库的时候，应对原数据进行深复制一份，以防数据修改之后，找不到原数据。
