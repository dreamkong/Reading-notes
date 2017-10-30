## 第一章 数据结构和算法

#### 将序列分解为单独的变量
 ```
 p = (4,5)
 a,b = p
 ```
 引申出任意长度的可迭代对象中分解元素

```
 *first,last = [x for x in range(1,100)]
 first,*middle,last = [x for x in range(1,100)]
 first,*last = [x for x in range(1,100)]
 
 # eg:
 record = ('ACME',50,123.45,(10,26,2017)
 name,*_,(*_,year) = record
 name
 'ACME'
 year
 2017
```


 
 
