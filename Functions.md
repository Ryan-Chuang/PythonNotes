# Functions
## Functions 主要内容
1. 定义函数  
2. 函数参数  
3. 递归  
4. 高阶函数（map、reduce、filter、sorted）  
5. 返回函数  
6. 匿名函数    
7. 装饰器  
8. 偏函数
## Functions概要  
### 1. 定义函数  
```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x, **2
```  
**有时需要在定义函数时，对输入参数进行类型检查，用isinstance()实现**  
### 2. 函数参数
```python
def f1(a, b, c=0, *args, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'd = ', d, 'kw =', kw)  
```  
**以上代码中：**  
**a和b是位置参数；**  
**c是默认参数，仅在不对其输入时起作用；**  
**args是可变参数：**
```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    
calc(1, 2, 3)
```
**需要注意的是，上述的```calc(1, 2, 3)```所传入的```1, 2, 3```是以tuple(1, 2, 3)的形式传给numbers的，在def calc的时候，如果需要调用某一个元素，可以采用tuple索引的方式，如numbers[0]表示1**  
**```*```分隔符后面的d是命名关键字参数，该参数必须传入参数名（在本小节开始的例子中，参数名是d）;**  
**kw是关键字参数;**
```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
    
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```  
### 3. 递归  
**递归函数定义时，需要考虑栈溢出问题，采用尾递归（函数返回时，调用自身，return语句不含有表达式）**  
```python  
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```
### 4. 高阶函数  
**函数名是一种变量，而且函数的参数可以接收变量**
#### map/reduce 函数
