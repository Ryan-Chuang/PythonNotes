# 面向对象编程主要内容
1. 类和实例
2. 访问限制
3. 继承和多态
4. 获取对象信息
5. 实例属性和类属性
# 面向对象编程的概念
+ 面向对象编程——Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。
+ 面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。
+ 而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。
# 类和实例
+ 在Python中，定义类是通过class关键字：
  ```python
  class Student(object):

    def __init__(self, name_instance, score_instance):
        self.name = name_instance
        self.score = score_instance

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
  ```
+ Student是类名，首字母大写。(object)表示该类是从哪个类继承下来的，如果没有合适的继承类，就可采用(object)类，这是所有类都会继承的类。
+ __init__是方法，方法的第一个参数总是self，表示创建的实例本身。创建实例的时候，self不需要传入。
# 访问限制
+ 为了使得内部属性不被外部访问，可以把属性的名称前加上两个下划线__。表示该变量是一个私有变量（private），只有内部可以访问。
```python
class Student(object):

    def __init__(self, name_instance, score_instance):
        self.__name = name_instance      # 以__开头的变量表示私有变量，外部无法访问
        self.__score = score_instance

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
        
    def get_name(self):                  # 在类的内部定义方法，以访问私有变量
        return self.__name

    def get_score(self):
        return self.__score
        
    def set_score(self, score):         # 在类的内部定义方法，以设置（修改）私有变量
        if 0 <= score <= 100:
          self.__score = score
        else:
          raise ValueError('bad score')
```
   + 如果不在类的内部采用私有变量，而通过外部设置类的变量，则比较容易传入无效的参数。
# 继承和多态
+ 继承是指在定义一个class的时候，可以从某个现有的class继承，新的类称为子类（Subclass），被继承的类称为基类、父类或超类（Base class、Super class）。
```python
class Animal(object):
    def run(self):
        print('Animal is running...')

class Dog(Animal):                  # 子类Dog继承了父类Animal的所有全部功能，如果Dog未定义run()方法，其拥有父类的run()方法
    def run(self):                  # 当子类和父类都存在相同的run()方法时，子类的run()方法覆盖了父类的run()方法：多态
        print('Dog is running...')
    def eat(self):
        print('Eating meat...')
def run_twice(animal):              # 外部定义函数，接收Animal类的参数。所有数据类型是Animal类的参数都可以传入run_twice()函数
    animal.run()
    animal.run()
>>> run_twice(Animal())
Animal is running...
Animal is running...
>>> run_twice(Dog())
Dog is running...
Dog is running...
```
+ 类是一种数据类型，与Python自带的str、list、dict等没有区别。
+ 对静态语言来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。
+ 对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了。
# 获取对象信息
+ type()
  + type()函数用于判断对象类型，返回对应的class类型。如：
    ```python
    >>> type(123) == type(456)
    True
    >>> type('abc') == str
    True
    ```
  + types模块中有一些常量可以判断对象是否是函数，如：
    ```python
    >>> import types
    >>> def fn():
            pass
    >>> type(fn) == types.FunctionType
    True
    >>> type(abs) == types.BuiltinFunctionType
    True
    ```
+ isinstance()
  + isinstance()函数可以判断class的类型。
  + 使用type()判断的基本类型也可以用isinstance()来判断。如可以判断变量是否是某些类型中的一种。如：
  ```python
  >>>isinstance('a', str)
  True
  ```
+ dir()
  + dir()函数用于获得一个对象的所有属性和方法。如：
  ```python
  >>> dir('ABC')
  ['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
  ```
  + 配合getattr()、setattr()以及hasattr()，可以直接操作一个对象的状态
# 实例属性和类属性
+ 由于Python是动态语言，因此根据类创建的实例可以任意绑定属性。如：
  ```python
  class Student(object):
    def __init__(self, name):
        self.name = name

  s = Student('Bob')
  s.score = 90
  print(s.name)
  print(s.score)
  ```
+ 写程序的时候，千万不要对实例属性和类属性使用相同的名字。
  
