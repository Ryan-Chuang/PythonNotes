# Modules主要内容
**1. 模块的概念**  
**2. 常用内建模块**  
**3. 常用第三方模块**  
# 模块的概念  
**模块就是```.py```文件，为了管理模块，Python提供了包（Package）的概念，包其实就是文件夹，但在文件夹下需存在```__init__.py```文件，该文件用于标识当前文件夹是一个包**  
```python
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ xyz.py
```
**__xxx__这样的变量是特殊变量，可以被直接引用，如```__author__ = SLZ```，一般不用这种变量；**  
**_xxx这样的函数或变量是非公开的，不应该被直接引用；**  
# 常用内建模块
## datetime  
**datetime模块中还包含一个datetime类**  
```python
>>> from datetime import datetime
>>> now = datetime.now()
```
## collections  
### namedtuple  
**namedtuple类似于C语言的结构，可以定义一种数据类型，具备tuple的不变形，且可以根据属性来引用**  
```python
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```  
### deque  
**deque是一种双向序列，支持从任意一段增加和删除元素**  
### defaultdict
**与dict类似，defaultdict在key不存在时，返回一个默认值**  
```python
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'
```
### OrderedDict  
**OrderedDict与dict的区别在于，前者保持了key的顺序**  
```python
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```
### ChainMap  
**ChainMap是一个dict，可以实现把一组dict串起来。常用的场景是将命令行参数dict、环境变量dict和默认参数dict的合并，用于传入参数时按照ChainMap中的顺序依次查找并传入参数。**  
### Counter  
**Counter是一个计数器，用来跟踪值出现的次数。**
```python
>>> c = Counter("abcdefgab")
>>> c["a"]
2
>>> c["c"]
1
>>> c["h"]
0
```
http://www.pythoner.com/205.html
