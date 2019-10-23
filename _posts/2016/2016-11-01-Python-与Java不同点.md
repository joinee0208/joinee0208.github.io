---
layout: post
title: Python-与Java不同点
categories: Python
original: true
description: Python-与Java不同点
keywords: Python
typora-root-url: ..\..
---

## 前提

Python语言和Java语言类似，都是高级语言，都是面向对象语言，都是解释执行语言，都是运行在虚拟机上，因此他们有很多共同点，当然也有不同点。

为了更好学习Python语言，在此拿Java语言作为对比，仅仅列出与Java不同的地方。

## 基本语法

### 文件类型

- 源代码：Java是`*.java` Python是`*.py`
- 字节码：Java是`*.class`  Python是`*.pyc`

### 命名规则

两种语言类似，都是采用驼峰命名法。

Python中的私有变量，私有方法以两个下划线作为前缀。

Python中的self相当于Java中的this。

### 代码缩进与冒号

Java中依靠`{}`就可以描述代码结构，Python就不需要靠`:`和`缩进`来描述代码结构，一个空格或几个制表符也符合缩进，但最好统一使用4个空格缩进。

### 模块导入

都有import语句。

Python多了以下语句：

- from...imprt...
- from...imprt...as...

### 注释

Java使用`// /**/`，Python使用 `#`表示注释。

python：

	- 中文注释 #-*- coding：UTF-8-*-
	- 跨平台注释 #！ /usr/bin/python

### 语句分隔

java必须使用`;`,Python可以使用也可以省略`;`。

两者都是使用`\`作为语句换行符。

### 全局变量

Python中的局部函数如果需要使用函数之外的全局变量，必须在使用前先用`global`关键字做一次引用申明，才能正确使用全局变量，不然会重新定义局部变量。

### 数据类型

Pythong中每个数值都是一个对象，同样的1在文件a和文件b中的id值一样。

Python中使用双单引号，双引号，双三引号来表示字符串，其中双三引号里的字符串可以不需要转义。

### 运算符

Python多了 x**y 来计算x的y次方值。
Python也可以用`<>`表示`!=`。
Python用 `and` 表示 `&&` 用 `or` 表示 `||` 用 `not` 表示`!`。
Pythong多了 `in`， `not in` ，  `is` ， `not is`等运算符。

## 语句控制

Python中的while循环格式如下：

	while(表达式):
		...
	else:
		...
当表达式为false时，运行else代码块。其中的else可以省略。

Python中的for循环格式如下：

	for 变量 in 集合:
		...
	else:
		...
集合可以是元组，列表，字典等，最后一次循环结束运行else代码块，其中的else可以省略。

另外Java中有如下循环格式

	for(表达式1;表达式2;表达式3)
		语句块
Python却不支持，如果想要上述效果，可以使用range()函数来实现。例如：

	for x in range(0,5,2)
		print x

	输出 0 2 4

## 内置数据结构

Python提供元组，列表，字典，序列等内置数据结构，这些是Python语言的精华。


### 元组

元组类似Java中的数组，都是一旦创建就不能修改长度，但Java中的数组类型必须一致，而Python中的元组类型可以不一样。

元组的创建格式如下：

	tuple_name=(元素1,元素2,...)

如果元组只有一个元素，则需要在元素1后加个逗号,不然会当成表达式：

	tuple_name=("test",)

负数索引从元组尾部开始计数，最尾端索引表示为-1，次尾端的索引为-2，依次类推。

可以分片获取元组的值：

	tuple_name[m:n]

Python中，将创建元组的过程称之为打包，相反，元组也可以执行解包操作：

	#打包
	tuple=("x","y","z","e")
	#解包
	a,b,c,d = tuple
	print a,b,c,d

### 列表

和元组类似，只不过是动态分配内存，因此可以添加，删除等操作。

列表的创建：

	list_name = [元素1,元素2,...]

列表模拟堆栈

	list = ["a","b","c"]
	list.append("d")
	print list
	print "弹出的元素:",list.pop()
	print list

列表模拟队列

	list = ["a","b","c"]
	list.append("d")
	print list
	print "弹出的元素:",list.pop(0)
	print list

在python中，还有一种列表推导式。

列表推导式由包含一个表达式的括号组成，表达式后面跟随一个 for 子句，之后可以有零或多个 for 或 if 子句。结果是一个列表，由表达式依据其后面的 for 和 if 子句上下文计算而来的结果构成。

例如，如下的列表推导式结合两个列表的元素，如果元素之间不相等的话:

	>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
	[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]


### 字典

字典是由键值对组成的集合，与Java中HashMap类类似。

字典的创建：

	dictionary_name = {key1:value1,key2:value2,...}

字典的访问：

	value = dict[key]

字典的添加，修改：

	dict["x"]="value"

全局字典-sys.modules模块，该字典是Python启动后就加载在内存中的，记录各个模块的引用，用以提高程序运行速度。

和列表推导式一样，也可以使用字典推导式。

### 序列

序列是具有索引和切片能力的集合，因此，元组，列表和字符串都属于序列。

## 模块与函数

Python的程序由包，模块，和函数组成。

### 包

包必须至少含有一个__init__.py文件，该文件的内容可以为空。__init__.py文件用于标识当前文件夹是一个包。

一般要在\_\_init__.py中定义一个\_\_all__列表，用来描述当前包所包括的模块：

	__all__ = [
	    'Environment', 'Template', 'BaseLoader', 
	]


### 模块

一个Python文件就是一个模块。

一个模块中可以包含函数和类：

	def func():
		print "MyModule.func()"
	
	class MyClass:
		def myFunc(self):
			print "MyModule.MyClass.myFunc()"

当导入一个模块时，Python首先查找当前路径，然后查找lib目录，site-packages目录和环境变量PYTHONPATH设置的目录。可以通过sys.path语句搜索模块的查找路径。

Java导入必须在类外，Python导入可以在任意位置。

模块有一些特殊属性：

	__name__ 模块的名称，经常用来和__main__比较，判断是否程序入口
	__doc__ 文档字符串的内容

Python有很多内置函数，可以在\_\_builtin__.py中查看。
 
### 函数

Python和Java一样，函数参数都是引用传递(Java基本数据类型还是值传递)。

注意函数参数默认值带来的问题，采用如下方式可以避免：

	def generate_new_list_with(my_list=None, element=None):
	  if my_list is None:
	    my_list = []
	  my_list.append(element)
	  return my_list

参数采用 `* args` 形式代表传入元组，采用`** args`形式表示传入字典。

### lambda函数

lambda函数用于创建一个匿名函数，函数名未和标识符进行绑定：

	lambda 变量1,变量2...：表达式  #一定是表达式，不能是语句

也可以进行绑定：

	func = lambda 变量1,变量2...：表达式
	#调用
	func()

还可以直接调用：

	print (lambda x:-x)(-2)

### Generator函数

和普通函数没啥区别，只是在函数体使用yield生成数据项即可：

	def 函数名(参数列表):
		...
		yield 表达式


## 面向对象

### 类和对象

类的定义:

	class class_name:
		...

类的方法至少有1个参数self，但是方法被调用时，可以不传递这个参数：

	class Fruit:
		def grow(self):
			print "grow"

Python对象的创建不像Java需要new关键字创建：

	fruit = Fruit()
	fruit.grow()

### 属性和方法

Python和Java Object基类类似，也有内置方法，比如：

- \_\_init\_\_表示类的构造方法，
- \_\_del\_\_类似C++中的析构函数。
- \_\_getattr\_\_获取属性的值
- \_\_setattr\_\_ 设置属性的值
- ...

属性前加\_\_表示私有，没有加表示公共：

	class Fruit:
		price = 0
		__color="red"
		def __init__(self):
			self.price = 10
			self.__color = "yellow"

类还提供了一些内置属性，例如\_\_dict__,\_\_bases\_\_,\_\_doc\_\_：

	fruit = Fruit()
	print Fruit.__bases__ #输出基类组成的元组 注意是类
	print fruit.__dict__  #输出属性组成的字典
	print fruit.__module__ #输出类所在的模块名
	print fruit.__doc__ #输出doc文档

此外还可以动态的给类增加方法：

	class Fruit:
		pass
	
	def add(self):
		print "grow"
	
	if __name == "__main__":
		Fruit.grow = add
		fruit = Fruit()
		fruit.grow()

### 继承

Python在类名后使用一对括号表示继承的关系，括号中的类即为父类。

	class Fruit:
		def __init__(self,color):
			self.color = color
	
		def grow(self):
			print "grow"
	
	class Apple(Fruit):
		def __init__(self,color):
			Fruit.__init__(self,color)

还可以使用super()调用父类的方法，但必须继承object类：

	class Fruit(object):
		def __init__(self,color):
			self.color = color
	
		def grow(self):
			print "grow"
	
	class Apple(Fruit):
		def __init__(self,color):
			super(Apple,self).__init__(self,color)

Python重写父类方法和Java类似。

### 抽象类

Python没有抽象类，因此只能模拟实现：

	def abstract():
		raise NotImplementedError("abstract")
	
	class Fruit:
		def __init__(self):
			if self.__class__ is Fruit:
				abstract()
	
	class Apple(Fruit):
		def __init__(self):
			Fruit.__init__(self)

### 接口

Python没有类似Java中的接口。

### 多重继承

Python和C++类似，支持多重继承，格式如下：

	class_name(parent_class1,parent_class2...)

注意：

	子类只会自动调用继承的第一个父类(即parent_class1)的初始化函数

### 运算符重载

Python和C++类似，支持运算符重载：

	class Fruit:
		def __init__(self,price=0):
			self.price = price
	
		def __add__(self,other): #重载加号运算符
			return self.price+other.price
		
		def __gt__(self,other): #重载大于运算符
			if self.price > other.price:
				flag = Ture
			else:
				flag = False
			return flag

	if __name__=="__main__":
		apple = Fruit()
		banana = Fruit()
		total = apple+banana
		print "total:",total

## 异常处理

Python和Java类似：

	try:
		resault = 10/0
	except ZeroDivisionError: #如果要获取异常实例，格式为：except ZeroDivisionError,arg:
		print "0 不能做除数"
	else:
		print resault

上述代码有个else代码块，作用是如果没有异常发生就会执行，还可以用finally。

	try:
		resault = 10/0
	except ZeroDivisionError:
		print "0 不能做除数"
	finally:
		print resault

上面两种区别是finally代码块始终会执行，而else代码块只会在执行成功后才会执行。

Python可以使用raise抛出异常：

	raise NameError

自定义异常需要按照命名规范以”Error“结尾，必须继承Exception类。


## 其它

### @符号
 
在Java中@表示注解，单纯标记一个Java语言元素。

Java自带了三个标注：
	
	@Override:只能用在方法之上的，用来告诉别人这一个方法是改写父类的。
	@Deprecated:建议别人不要使用旧的API的时候用的,编译的时候会用产生警告信息,可以设定在程序里的所有的元素上.
	@SuppressWarnings:这一个类型可以来暂时把一些警告信息消息关闭.

当然用户也可以通过@interface来自定义注解。

在运行中可以通过classObject.getAnnotation(x)来获取注解对象。

在Python中@表示装饰器，和Java的注解完全不一样，作用是可以不改变原有代码结构的前提下新增业务需求。

比如：

	import time
	def foo():
	    print 'in
	 foo()'
	
	#定义一个计时器，传入一个，并返回另一个附加了计时功能的方法
	def timeit(func):
		#定义一个内嵌的包装函数，给传入的函数加上计时功能的包装
	    def wrapper():
	        start= time.clock()
	        func()
	        end=time.clock()
	        print 'used:',end - start
	     
	    #将包装后的函数返回
	    return wrapper
	 
	foo= timeit(foo)
	foo()

Python @符号的作用类似于foo= timeit(foo)。

Python也有内置的三个装饰器：
 
	@staticmethod 作用是把类中定义的实例方法变成静态方法
	@classmethod 作用是把类中定义的实例方法变成类方法
	@property 作用是把类中定义的实例方法变成类属性

### 文档字符串

在Java中一般都是在类或者方法、字段前遵守Javadoc规范，在使用javadoc工具生成html文档。

javadoc标记如下：

	/**
		@author 标明开发该类模块的作者 
		@version 标明该类模块的版本 
		@see 参考转向，也就是相关主题 
		@param 对方法中某参数的说明 
		@return 对方法返回值的说明 
		@exception 对方法可能抛出的异常进行说明 
	*/

在python中，在def一个方法的下一行，使用三引号来描述一个方法解释。

python文档字符串如下：

	>>> def my_function():
	...     """Do nothing, but document it.
	...
	...     No, really, it doesn't do anything.
	...     """
	...     pass
	...
	>>> print my_function.__doc__
	    Do nothing, but document it.
	
	    No, really, it doesn't do anything.


