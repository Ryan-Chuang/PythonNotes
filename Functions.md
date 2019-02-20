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
1. 定义函数  
```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x, **2
```  
**有时需要在定义函数时，对输入参数进行类型检查，用isinstance()实现**  
2. 函数参数
```python
def f1(a, b, c=0, *args, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'd = ', d, 'kw =', kw)  
```  
**以上代码中：  
**a和b是位置参数；  
c是默认参数，仅在不对其输入时起作用；  
args是可变参数：
```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    
calc(1, 2, 3)
```
