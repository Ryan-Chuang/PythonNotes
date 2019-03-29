# python 类相关



# 调试相关
  + MATLAB中有变量空间，可以在运行完毕，发现图画错了的时候，不必重新运行程序即可进行调试。
  + Python没有这个变量空间，可以采用将变量保存下来，然后调用，再画图的方式实现这一操作。
  + 首先需要 ```python -m pip install dill```模块，然后可以参考如下例子：  
  ```python
  import numpy as np
  import dill
 
  T='Hiya'
  val=[1,2,3]
  a = np.zeros([4,5])
  # save the data 
  filename= 'globalsave.pkl'
  dill.dump_session(filename)

  # load the session again
  dill.load_session(filename)
  print(a)
  ```
