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
