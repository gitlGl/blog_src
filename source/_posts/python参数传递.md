---
title: python参数传递
date: 2022-12-27 17:06:24
tags: python基础
categories: python
---
__引用其实是一个类似指针的变量，参数传递的是复制的变量（引用）__ <!--more-->
```python
class test():
    test = 1
def t(object,int_):
    print(id(int_))
    object.test = 2#解引用，
    object = test()##改变引用的值，该引用与外部引用无关，object类似指针，指向生成的对象的地址
    int_ = 5##改变引用的值，该引用与外部引用无关,变量int_类似指针，指向5的地址
    print(id(int_))

text = test()
print(id(4))
int_ =4#变量int_d值是4的地址
print(id(int_))
t(text,int_)#参数传递是复制变量text,int_，变量里面的值是test（）对象、4的地址.
```
> 程序输出的id地址如下：
1410117920
1410117920
1410117920
1410117952

> python 把参数传递统一为值传递，只不过传递的值（形参）是实参的地址。形参也是一个变量，变量的值是实参的地址，可以改形参变量的值，使它不再指向实参。网络上的可变类型与不可变类型解释不准确，所谓不可变类型只是变量是const而已，可变类型变量是非const。