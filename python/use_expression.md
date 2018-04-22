## 在python中使用正则表达式
Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。
re 模块使 Python 语言拥有全部的正则表达式功能。
下面主要介绍一下re模块的使用：
### re.match函数
re.match 尝试从字符串的起始位置匹配一个模式，匹配成功就返回一个正则表达式匹配对象，如果不是起始位置匹配成功的话，就返回none。

函数语法：
```Python
re.match(pattern, string, flags=0)
```
|参数|说明|
|:-----:|:----:|
|pattern|正则表达式匹配模式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。|
案例：
```Python
import re

username = 'jackfrued'
m = re.match(r'\w{6,20}', username)  # 从开始位置匹配长度为6到20的字母数字下划线或者汉字
n = re.match(r'\d{6,20}', username)  # 匹配长度为6到20的数字
print(m)
print(n)
print(m.span())  # 取出匹配成功的位置，如果没有匹配到则是None
```
输出结果如下：
```
<_sre.SRE_Match object; span=(0, 9), match='jackfrued'> # 匹配到的正则表达式对象
None
(0, 9) 
```
>**说明：**正则表达式对象的span()方法可以返回一个元组，这个元组就是正则表达式在该字符串中匹配成功的位置。

### re.search函数

re.search()尝试从字符串的任意位置匹配一个模式，匹配成功，则返回一个正则表达式匹配对象，否则返回None。

函数语法如下：
```Python
re.search(pattern, string, flags)
```
search()函数和match()的使用方法类似，唯一的区别就是match()函数只能从字符串的开始位置匹配成功才返回匹配对象，否则为None，而search()可以从任意位置匹配，直到第一次匹配到。
案例：
```Python
import re

username = '###jackfrued###'
m = re.search(r'\w{6,20}', username)  # 从开始位置匹配长度为6到20的字母数字下划线或者汉字
n = re.search(r'\W{6,20}', username)  # 匹配长度为6到20的任意非字母数字下划线和汉字
print(m)
print(n)
print(m.span())  # 取出匹配成功的位置，如果没有匹配到则是None

```
输出结果如下：
```
<_sre.SRE_Match object; span=(3, 12), match='jackfrued'>
None
(3, 12)
```
### re.findall函数

re.findall()尝试从整个字符串查找所有满足查找条件的字符，并放在一个列表中返回，如果没有匹配到则返回一个空列表。

函数使用方法与match和search类似，语法如下：
```Python
re.findall(pattern, string, flags)
```
案例：
```Python
import re

username = '###jackfrued###'
list1 = re.findall(r'[ion]', username)
list2 = re.findall(r'[ace]', username)
print(list1)
print(list2)
```
输出结果如下：
```
[]
['a', 'c', 'e']
```
### re.split函数

re.split()函数表示将字符串按照一定匹配模式进行切割，切割后返回一个列表。

函数语法如下：
```Python
re. split ( pattern, string, maxsplit=0, flags=0 )
```

|参数|说明|
|:------:|:----:|
|pattern|正则表达式匹配模式|
|string|要匹配的字符串|
|maxsplit|指定最大的切割次数，默认为0,表示不限制|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。|

案例：
```Python
import re

sentence1 = '我的电话是18912901290不是18919801980'
list1 = re.split('是', sentence1)
list2 = re.split('是', sentence1, 1)
print(list1)
print(list2)
```
输出结果如下：
```
['我的电话', '18912901290不', '18919801980']
['我的电话', '18912901290不是18919801980']
```
### re.sub函数

re.sub()函数表示将字符串按照一定匹配模式进行切割，切割后返回一个列表。

函数语法如下：
```Python
re. sub ( pattern, repl, string, count=0, flags=0 )
```

|参数|说明|
|:------:|:----:|
|pattern|需要替换字符串的正则表达式模式|
|repl|替换的字符串|
|string|要匹配的字符串|
|count|指定最大的切割次数，默认为0,表示替换所有|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。|

案例：
```Python
import re

sentence1 = '我的电话是18912901290不是18919801980'
m = re.sub(r'189', '028-', sentence1)
n = re.sub(r'189', '028-', sentence1, 1)
print(m)
print(n)
```
输出结果如下：
```
我的电话是028-12901290不是028-19801980
我的电话是028-12901290不是18919801980
```

>**注意：**repl要替换的字符串，可以是一个函数，表示用函数的返回值去替换。

### re.compile函数

re.compile()函数函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，给其他需要用到正则表达式对象的函数使用。

函数语法如下：
```Python
re. complie ( pattern, flags=0 )
```

|参数|说明|
|:------:|:----:|
|pattern|需要替换字符串的正则表达式模式|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。|

案例：
```Python
import re

pattern1 = re.compile(r'\d{2}')
pattern2 = re.compile(r'我')
sentence1 = '我的电话是18912901290不是18919801980'
m = re.sub(pattern1, '028-', sentence1)
list1 = re.split(pattern1, sentence1)
match_m = re.match(pattern2, sentence1)
search_m = re.search(pattern1, sentence1)
print(m)
print(list1)
print(match_m)
print(search_m)
```
输出结果如下：
```
我的电话是028-028-028-028-028-0不是028-028-028-028-028-0
['我的电话是', '', '', '', '', '0不是', '', '', '', '', '0']
<_sre.SRE_Match object; span=(0, 1), match='我'>
<_sre.SRE_Match object; span=(5, 7), match='18'>
```
### 常用正则表达式对象的方法
|参数|使用方法| 解释 |
|:-------:|:-------:|:-------:|
|match| pattern.match(string, pos=0, endpos=-1) |pos指定匹配开始位置，默认为o<br>endpos指定匹配结束位置，默认为-1|
|search| pattern.search(string, pos=0, endpos=-1) |pos指定匹配开始位置，默认为o<br>endpos指定匹配结束位置，默认为-1|
|findall| pattern.findall(string, pos=0, endpos=-1) |pos指定匹配开始位置，默认为o<br>endpos指定匹配结束位置，默认为-1|
|split| pattern.split(string, maxsplit=0)| maxsplit指定切割次数，默认为0表示不限制|
|sub| pattern.sub(repl, string, count=0)|repl表示替换的字符串(可以是一个函数)<br>count表示匹配次数，默认为0，表示替换所有|

正则表达式对象的可使用的方法其实和re模块的那几个函数差不多，只是对于某些方法支持指定匹配的位置。

### 标志位
正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位 OR(|) 它们来指定。
常用的标志修饰符见下表：

|修饰符|描述|
|:-------:|:------:|
|re.I|	使匹配对大小写不敏感|
|re.L|做本地化识别（locale-aware）匹配|
|re.M|	多行匹配，影响 ^ 和 $|
|re.S|	使 . 匹配包括换行在内的所有字符|
|re.U|	根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.|
|re.X|	该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。|

### 练习一：匹配电信、移动、联通的号码
```Python
import re


def main():
    your_number = input('请输入您的号码：')
    telecom = re.compile('^1(?:[35]3|8[019]|77)\d{8}$')
    unicom = re.compile('^1(?:3[012]|5[56]|8[56]|76|45)\d{8}$')
    mobile = re.compile('^1(?:3[4-9]|5[012789]|8[23478]|47|78)\d{8}')
    test1 = re.match(unicom, your_number)
    test2 = re.match(telecom, your_number)
    test3 = re.match(mobile, your_number)
    if test1:
        print('你的号码是联通号码')
    elif test2:
        print('你的号码是电信号码')
    elif test3:
        print('你的号码是移动号码')
    else:
        print('你输入的号码错误，请重新输入。')


if __name__ == '__main__':
    main()

```
### 练习二：将某段字符串中的数字替换为当前数字的平方
```Python
import re

str1 = '我有50元钱，今天花了2两块坐公交，30块钱吃饭，还剩18.'


def foo(num):
    num = num.group("name")
    num = int(num)
    return str(num ** 2)


m = re.sub(r'(?P<name>\d+)', foo, str1)
print(str1)
print(m)
```
程序执行结果如下：
```
我有50元钱，今天花了2两块坐公交，30块钱吃饭，还剩18.
我有2500元钱，今天花了4两块坐公交，900块钱吃饭，还剩324.
```
## 总结
re 模块的一般使用步骤如下：
- 使用 compile 函数将正则表达式的字符串形式编译为一个 Pattern 对象；
- 通过 Pattern 对象提供的一系列方法对文本进行匹配查找，获得匹配结果（一个 Match 对象）；
- 最后使用 Match 对象提供的属性和方法获得信息，根据需要进行其他的操作；

Python 的正则匹配默认是贪婪匹配。
