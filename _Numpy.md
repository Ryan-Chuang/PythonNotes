# Numpy函数

## numpy.where()
### 给定一个array，找其中某一个值的index：
    import numpy as np
    lbd_min = 1.51
    lbd_max = 1.59
    lbd_target = 1.55
    lbd_step = 10**-6
    lbd0 = np.round(np.arrange(lbd_min, lbd_max, lbd_step), 6)    # 用round是为了避免精度问题
    lbd_target_index = np.where(lbd0 == lbd_target)[0][0]         # np.where()返回的是tuple,第一个[0]取tuple的array，第二个[0]取array的数值。
    print(lbd0[lbd_target_index])
    
