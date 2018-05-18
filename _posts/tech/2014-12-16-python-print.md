---
layout: post
title: Python里的print用法总结
category: 编程与开发
tags: Python
keywords: Python,print
---

在Python中接受来自用户的输入很简单，可以使用input，它类似于C语言里的 scanf 或者C++中的 cin。

```python
>>> x = input("Please input x:")

Please input x:10

>>> print(x)
10
```

用于输出的print，其一般用法是比较 straightforward 的，就像上面的print(x)那样，对于一个已知的对象（无论什么类型），Python都能自行输出。

一个比较tricky的问题是，如何做“格式化”输出。方法跟C语言差不多，也是使用%，但是变量之前还要再写一个%，这个跟C语言里面略有不同（C里面还需要一个逗号）。来看一些例子：

```python
>>> a = 10

>>> print("The value is %d" %a)
The value is 10

>>> print("The value is %d" %10)
The value is 10

>>> s = 'hello'
>>> x = len(s)

>>> print("The length of %s is %d" % (s,x))
The length of hello is 5
```

关于格式化输出的一些技巧总结：


1、%字符：标记转换说明符的开始

2、转换标志：<br>

- `-`表示左对齐；
- `+`表示在转换值之前要加上正负号；
- `空白字符`表示正数之前保留空格；
- `0`表示转换值若位数不够则用0填充

3、最小字段宽度：转换后的字符串至少应该具有该值指定的宽度。如果是`*`，则宽度会从值元组中读出。

4、点`.`后跟精度值：如果转换的是实数，精度值就表示出现在小数点后的位数。如果转换的是字符串，那么该数字就表示最大字段宽度。如果是`*`，那么精度将从元组中读出

5、字符串格式化转换类型

转换类型      |含义
-------------|----------------
d,i          |带符号的十进制整数
o            |不带符号的八进制
u            |不带符号的十进制
x            |不带符号的十六进制（小写）
X            |不带符号的十六进制（大写）
e            |科学计数法表示的浮点数（小写）
E            |科学计数法表示的浮点数（大写）
f,F          |十进制浮点数
g            |如果指数大于-4或者小于精度值则和e相同，其他情况和f相同
G            |如果指数大于-4或者小于精度值则和E相同，其他情况和F相同
C            |单字符（接受整数或者单字符字符串）
r            |字符串（使用repr转换任意python对象)
s            |字符串（使用str转换任意python对象）

来看一些例子：

```python
>>> pi = 3.141592653

>>> print('%10.3f' % pi) #字段宽10，精度3
     3.142

>>> print("pi = %.*f" % (3,pi)) #用*从后面的元组中读取字段宽度或精度
pi = 3.142

>>> print('%010.3f' % pi) #用0填充空白
000003.142

>>> num = -0.15

>>> print('%+f' % num) #显示正负号
-0.150000 
```

以及

```python
print ("Name:%-10s Age:%-8d Height:%-8.2f"%("Tom", 18, 1.83)) #指定占位符宽度（左对齐）
Name:Tom        Age:18       Height:1.83    

print ("Name:%10s Age:%8d Height:%8.2f"%("Tom", 18, 1.83))  #指定占位符宽度
Name:       Tom Age:      18 Height:    1.83

print ("Name:%-10s Age:%08d Height:%08.2f"%("Tom", 18, 1.83)) #指定占位符（用0当占位符）
Name:Tom        Age:00000018 Height:00001.83

print ("Name:%-10s Age:% 8d Height:%-8.2f"%("Tom", 18, 1.83)) #空白字符表示正数之前保留空格
Name:Tom        Age:      18 Height:1.83
```

最后，print总是默认换行的，例如：

```python
>>> for x in range(0,5):
        print(x)
    
0
1
2
3
4
```

如果想要不换行，对于 Python 3.x 来说，你需要把语句写成 `print(x，end = '' )`

```python
>>> for x in range(0,5):
        print (x,end = '')
    
01234
```

> 本文中之例子皆基于Python 3.6完成。笔者所使用的IDE是Spyder 3.1.4。


<span style="color:blue">**（本文完）**</span>


