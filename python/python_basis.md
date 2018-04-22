
## 概述

- 是一种面向对象的解释型计算机程序设计语言（解释一句执行一句，跨平台可移植性高，执行效率低。），由荷兰人Guido van Rossum于1989年发明，第一个公开发行版发行于1991年。Python是纯粹的自由软件， 源代码和解释器CPython遵循 GPL(GNU General Public License)协议。Python语法简洁清晰，特色之一是强制用空白符(white space)作为语句缩进。

- [python官网](https://python.org)，下载安装程序，及查看相关文档

>**说明**：Linux环境先更新到Python 3.X版本是需要用到源代码安装

- python包管理工具pip安装管理第三方模块：

第一种方法：
```shell
python -m pip install ipython jupyter
```
第二种方法：
```Shell
pip install ipython jupyter
```
- 第一个Python程序 - hello world

```Python

"""

第一个Python程序

Version:1.0
Author:Zhang
Date:*****
Modifier:******

"""
#使用了Python内置的print函数打印字符

print('hello world')
```
- 交互式环境进行Python开发:可以使用ipython或者jupyter的notebook项目
```Python
jupyter notebook
```

- 团队开发，以及使用多文件多模块协作的大项目，使用[PyCharm](https://www.jetbrains.com/pycharm/).



# Python开发思想(Python之禅)

- Python开发中应该注意的事项：



```python
import this
```

    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    

```Python
"""
优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）

即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）

不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
 
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
 
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
 
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
 
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）
"""
```

## 变量

1.变量的作用

>变量是数据的载体，是一段用于存放数据的内存空间。

2.变量的命名规则：

>1.由字母数字下划线组成，不能以数字开头，不能使用特殊字符

>2.不能和关键字，保留字冲突

>3.大小写敏感

>官方规范标准建议(PEP8)：命名用全小写字母，多个单词用下划线连接起来，变量名长度没有限制（最好不要超过一行代码的字符：79个）
(一行代码不要超过79个字符)

3.变量的类型
>整型(int)，浮点型(float)，布尔型(bool)，字符型(string)，复数型(complex)

>变量类型转换:对变量调用对应的方法即可实现转换，如下程序，将整型转换为字符型



```python
a = 1
print(type(a))
a = str(a)
print(type(a))
```

    <class 'int'>
    <class 'str'>
    

>**说明**：字符转ASCII使用ord(),ASCII转字符使用chr()


```python
print('A的ASCII码为：',ord("A"))
print('98对应的字符是：', chr(98))
```

    65
    b
    


## 运算符

- 赋值运算符

![比较运算符](http://upload-images.jianshu.io/upload_images/10930505-3baf2674e5ce801a..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 比较运算符

![比较运算符](http://upload-images.jianshu.io/upload_images/10930505-bda65238daa02d6b..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 算术运算符

![算术运算符](http://upload-images.jianshu.io/upload_images/10930505-9684367c7453ecbd..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 逻辑运算符

![逻辑运算](http://upload-images.jianshu.io/upload_images/10930505-075a969a6f3560f2..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 身份运算符

![身份运算符](http://upload-images.jianshu.io/upload_images/10930505-60c918dd78998d26..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 成员运算符

![成员运算符](http://upload-images.jianshu.io/upload_images/10930505-a102321543ef7a55..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>**说明：**在实际开发中应该避免使用浮点数进行计算，在涉及到浮点数运算时，应尽量将浮点数运算转换为整数运算。


## 分支结构

```Python
if 条件:
    执行语句...
else:
    执行语句...
```
或者
```Python
if 条件1:
    执行语句...
elif 条件2:
    执行语句...
else:
    执行语句
```
python 不支持 switch , case 语句，所以多个条件判断，只能用 elif 来实现，如果判断需要多个条件需同时判断时，可以使用 “and” “or” “not”来实现。 使用or（或）时，表示两个条件有一个成立时判断条件成功；使用 and （与）时，表示只有两个条件同时成立的情况下，判断条件才成功。

## 循环结构

```Python
for i in 条件:
    循环语句...
else:
    循环正常退出后执行语句...
```
或者
```Python
while 循环条件:
    循环语句...
else:
    循环正常退出后执行语句
```
for循环可以用来遍历任何序列的项目，在没有遇到break的情况下会把这个项目中的第一个元素到最后一个元素依次访问一次，如果for后面有else语句，结束循环之后就执行else语句。

while语句条件表达式的值是布尔型，表达式的值为“真”或者“假”决定了循环继续或者停止。 
while语句的执行过程是：每一次循环之前计算机先判断条件表达式的值，如果其布尔值为真，就执行循环体，如此反复执行，直到条件表达式的值为布尔假，就结束循环。如果while后面有else语句，结束循环之后就执行else语句。

>1.while语句的条件表达式不需要用括号括起来，表达式后面必须有冒号。循环体不需要用大括号括起来。
2.python与其他大多数语言不同，在for循环和while循环中可以使用else语句。
3.while适合用在不确定循环次数的循环结构中，for适用于确定循环次数的循环，其中for循环的速度优于while循环。
4.range()函数可以用来生成指定步长的数字序列，len()函数用来

## 本周重点掌握程序：

## （一）画一个超正方体


```python
import turtle
dadian = [(0, 0), (200, 0), (240, 60), (40, 60), (0, 200), (200, 200), (240, 260), (40, 260)]
xiao = [(60, 50), (160, 50), (180, 80), (80, 80), (60, 150), (160, 150), (180, 180), (80, 180)]

#画小圆点
def huadian():
    turtle.pendown()
    turtle.dot(10, "black")
    turtle.penup()

#移动画笔到各个点上，并画出小圆点
def daodian():
    for i in range(8):
        turtle.penup()
        turtle.goto(dadian[i])
        huadian()
        turtle.goto(xiao[i])
        huadian()

#画两个正方体的所有的横线
def hengxian():
    for x in (dadian, xiao):
        i = 0
        while i<8:
            turtle.goto(x[i])
            turtle.pendown()
            turtle.goto(x[i+1])
            turtle.penup()
            i += 2

#画所有的正方体的斜线
def xiexian():
    for x in (dadian, xiao):
        turtle.goto(x[1])
        turtle.pendown()
        turtle.goto(x[2])
        turtle.penup()
        turtle.goto(x[3])
        turtle.pendown()
        turtle.goto(x[0])
        turtle.penup()
        turtle.goto(x[5])
        turtle.pendown()
        turtle.goto(x[6])
        turtle.penup()
        turtle.goto(x[7])
        turtle.pendown()
        turtle.goto(x[4])
        turtle.penup()

#画两个正方体所有的竖线
def shuxian():
    for x in (dadian, xiao):
        i = 0
        while i<4:
            turtle.goto(x[i])
            turtle.pendown()
            turtle.goto(x[i+4])
            turtle.penup()
            i += 1

#依次连接两个正方形的顶点
def lianjiexian():
    for i in range(8):
        turtle.goto(dadian[i])
        turtle.pendown()
        turtle.goto(xiao[i])
        turtle.penup()

#画超正方体的函数
def main():
    turtle.showturtle()
    turtle.speed(5)
    turtle.pensize(4)
    turtle.pencolor("blue")
    daodian()
    hengxian()
    xiexian()
    shuxian()
    lianjiexian()
    turtle.done()

main()
```

##  捕鱼和分鱼问题：



```python
"""
Ａ、Ｂ、Ｃ、Ｄ、Ｅ五个人在某天夜里合伙去捕鱼，到第二天凌晨时都疲惫不堪，于是各自找地方睡觉。
日上三杆，Ａ第一个醒来，他将鱼分为五份，把多余的一条鱼扔掉，拿走自己的一份。Ｂ第二个醒来，
也将鱼分为五份，把多余的一条鱼扔掉，拿走自己的一份。Ｃ、Ｄ、Ｅ依次醒来，也按同样的方法拿走
鱼。问他们合伙至少捕了多少条鱼？
"""

x = 1   #假设最后一个人拿走x调鱼走，设置循环的初始值
while True:
    y = x  #将x的值赋给y以便于用y去计算前边的人拿走的鱼，同时也不破坏x的累加
    for i in range(4):   #从第一个人拿走计算到最后一个人拿走鱼只进行了4次分鱼
        if (  (  (5 * y) + 1 ) % 4 == 0):    #循环确保每个人拿走的鱼都是整数
                y = (  (5 * y) + 1 ) / 4     
                z = 1    #设置跳出外层循环的开关
        else:
            z = 0    #如果for循环未能执行4次则不能跳出外层while循环
            break   #确保for循环能够执行4次
    if z == 1:
        print("结果为：", 5 * int(y) + 1 )   #经过以上循环赋值后得出的y是第一个人拿走的条数，所以总共有的鱼就是5*y+1
        break
    else:
        x += 1  #穷举最后一个人拿走鱼的条数
```

    结果为： 3121
    

## SCAPS赌博游戏


```python
"""
craps赌博游戏

规则：玩家掷两个骰子，每个骰子点数为1-6，如果第一次点数和为7或11，则玩家胜；
如果点数和为2、3或12，则玩家输庄家胜。若和为其他点数，则记录第一次的点数和，
玩家继续掷骰子，直至点数和等于第一次掷出的点数和则玩家胜；若掷出的点数和为7
则庄家胜。

Author:
Version:1.0
Date:18/03/02
Modified:

"""

from random import randint

#生成两个1到6的随机整数
def shake_child():
    your_point1 = randint(1, 6)
    your_point = randint(1, 6)
    total_point = your_point1 + your_point
    return total_point, your_point1, your_point

#游戏输赢判断，及结果显示，以及出现胜者时玩家点数的显示
def games():
    total_point, your_point1, your_point = shake_child()
    if total_point in (7, 11):
        print("玩家点数为：", your_point, your_point1)
        print("玩家胜出！")
        return 1
    elif total_point in (2, 3, 12):
        print("玩家点数为：", your_point, your_point1)
        print("庄家胜！")
        return 0
    else:
        first_total_point = total_point
        print("第一次玩家点数为：", your_point, your_point1)
        while True:
            next_point, your_point1, your_point = shake_child()
            if next_point == 7:
                print("玩家点数为：", your_point, your_point1)
                print("玩家输，庄家胜！")
                return 0
                break
            if next_point == first_total_point:
                print("玩家点数为：", your_point, your_point1)
                print("庄家输，玩家胜！")
                return 1
                break

#游戏双方玩家初始值设置，每局赌注的设置，游戏结束和主动退出游戏的设置，双方剩余金币的显示
#赌注是否符合规定的设置：（赌注不得大于双方中少的哪一个，且必须为正整数，否则要重新输入）
def game_main():
    com_bets = 3000
    your_bets = 3000
    print("电脑的剩余金币为：" + str(com_bets) + "\n您的剩余金币为：" + str(your_bets))
    while True:
        betting = eval(input("您的下注金额是多少(0:退出游戏)？"))
        if betting == 0:
            break
        while betting > com_bets or betting > your_bets or betting < 0 or betting % 1 != 0:
            betting = eval(input("下注金额错误，请重新输入下注金额：(0:退出游戏)？"))
        if games() == 1:
            com_bets -= betting
            your_bets += betting
        else:
            com_bets += betting
            your_bets -= betting
        print("电脑的剩余金币为：" + str(com_bets) + "\n您的剩余金币为：" + str(your_bets))
        if com_bets == 0:
            print("恭喜你，获得最终的胜利！")
            break
        if your_bets == 0:
            print("厉害了，输得一干二净！")
            break

#开始程序：
game_main()
```

    电脑的剩余金币为：3000
    您的剩余金币为：3000
    您的下注金额是多少(0:退出游戏)？500
    第一次玩家点数为： 4 6
    玩家点数为： 3 4
    玩家输，庄家胜！
    电脑的剩余金币为：3500
    您的剩余金币为：2500
    您的下注金额是多少(0:退出游戏)？2501
    下注金额错误，请重新输入下注金额：(0:退出游戏)？2500
    玩家点数为： 1 1
    庄家胜！
    电脑的剩余金币为：6000
    您的剩余金币为：0
    厉害了，输得一干二净！
    

## 总结

感谢老师的辛勤付出，因为前期自学过Python，因此第一周的内容在写程序时用了函数来解决问题，个人感觉第一周的学习很轻松，也很愉悦，同时也学到了很多自学时没有注意到的东西。正所谓“人生苦短，我用Python”，我的Python之路才刚刚开始，相信明天会更好。