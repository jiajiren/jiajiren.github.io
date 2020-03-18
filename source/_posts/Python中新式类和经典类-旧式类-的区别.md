---
title: Python中新式类和经典类(旧式类)的区别
date: 2019-12-24 10:18:54
tags:
	- Python
---
Python2.x中，默认都是经典类，只有显式继承了object的才是新式类，即：

    class Person(object):pass    新式类
    class Person():pass    经典类

在Python 3.x中取消了经典类，默认都是新式类，并且不必显式的继承object，也就是说：

    class Person(object):pass    新式类
    class Person():pass     新式类

`新式类`和`经典类（旧式类）`的`区别`的在于`子类多继承`的情况下，`经典类`多继承搜索顺序是`深度优先`，`新式类`多继承搜索顺序是`广度优先`。

具体代码示例如下：

**1. 经典类-- 深度优先**

```
class Person():
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Person.")


class Man(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Man.")


class Woman(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Woman.")


class Child(Man,Woman):
    def __init__(self):
        pass


child = Child()
child.say_hello()
```
输出为：  `Hello, I'm Man.`   即调用的是Man中的say_hello()方法。

注释掉Man中的say_hello()方法我们再次执行：
```
class Person():
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Person.")


class Man(Person):
    def __init__(self):
        pass
    # def say_hello(self):
    #     print("Hello, I'm Man.")


class Woman(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Woman.")


class Child(Man,Woman):
    def __init__(self):
        pass


child = Child()
child.say_hello()
```
这是输出的是：`Hello, I'm Person.`  即执行的是Man的父类Person的say_hello()方法。

也就说Man中无此方式时，优先去查找Man的父类Person中的方法，也就证明了多继承的情况下，经典类是`深度优先`的。


**2. 新式类-- 广度优先**

```
#这里基类Person显式的继承了object
class Person(object):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Person.")


class Man(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Man.")


class Woman(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Woman.")


class Child(Man,Woman):
    def __init__(self):
        pass

child = Child()
child.say_hello()
```
这时的输出为：`Hello, I'm Man.` ，即执行的是Man中的say_hello()方法。

然后我们将Man中的say_hello()方法注释掉，再次执行：
```
#这里基类Person显式的继承了object
class Person(object):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Person.")


class Man(Person):
    def __init__(self):
        pass
    # def say_hello(self):
    #     print("Hello, I'm Man.")


class Woman(Person):
    def __init__(self):
        pass
    def say_hello(self):
        print("Hello, I'm Woman.")


class Child(Man,Woman):
    def __init__(self):
        pass

child = Child()
child.say_hello()
```
这是的输出为：`Hello, I'm Woman.` ，即执行的Woman中的say_hello()方法。

也就是当Man中无此方法时，优先的去Woman中查找，即`广度优先`。