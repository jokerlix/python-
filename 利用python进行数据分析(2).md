# 利用python进行数据分析

>> pandas 对象的重要方法 reindex 
>>> 作用是创建一个新对象 数据符合新的索引


``` python
In [1]: import pandas as pd                                                                                     

In [2]: obj = pd.Series([4.5,7.2,-5.3,3.6],index = ['d','b','a','c'])   
In [4]: obj  
Out[4]: 
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64

```

>> 使用 `Series` 的 `reindex` 根据新的索引重新排列 
>>> 如果索引不存在 插入 `NaN`

``` python
In [5]: obj2 = obj.reindex(['a','b','c','d','e'])                                                               

In [6]: obj2                                                                                                    
Out[6]: 
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64
```
#### 插值处理 

`method` 方法可以实现重新索引时候的一些插值处理
``` python
In [7]: obj3 = pd.Series(['blue','purple','yellow'],index = [0,2,4])                                            

In [8]: obj3                                                                                                    
Out[8]: 
0      blue
2    purple
4    yellow
dtype: object

In [9]: obj3.reindex(range(6),method='ffill')                                                                   
Out[9]: 
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object

In [10]: obj3.reindex(range(6),method='bfill')                                                                  
Out[10]: 
0      blue
1    purple
2    purple
3    yellow
4    yellow
5       NaN
dtype: object

```

此处 `method` 方法 有 `ffill` 和 `bfill` 前向填充 后向填充

#### reindex 结合 DataFrame

借助  `DataFrame` `reindex` 可以修改（行索引） 和 列 只传递一个序列时，会重新索引结果的行

``` python 
In [12]: frame = pd.DataFrame(np.arange(9).reshape((3,3)),index = ['a','c','d'],columns=['Ohio','Texas','Califor
    ...: nia'])                                                                                                 

In [13]: frame                                                                                                  
Out[13]: 
   Ohio  Texas  California
a     0      1           2
c     3      4           5
d     6      7           8

In [14]: frame2 = frame.reindex(['a','b','c','d'])                                                              

In [15]: frame2                                                                                                 
Out[15]: 
   Ohio  Texas  California
a   0.0    1.0         2.0
b   NaN    NaN         NaN
c   3.0    4.0         5.0
d   6.0    7.0         8.0

In [16]: states = ['Texas','Utah','California']                                                                 

In [17]: frame.reindex(columns=states)                                                                          
Out[17]: 
   Texas  Utah  California
a      1   NaN           2
c      4   NaN           5
d      7   NaN           8

```

#### reindex 方法参数（具体可以查pandas手册）

<br>index                新建作为索引序列 可以索引任意类型 实例或者其他序列型的python 数据结构 索引使用时无须复制
<br>method               插值方式  `ffill`  前向填充  `bfill` 后向填充
<br>fill_value           通过重新索引缺失数据时候 使用的替代值
<br>limit                当前向或者后向填充的时候 最多的元素个数
<br>tolerance            当前向或者后向填充的时候 最大的数字距离
<br>level                匹配MultiIndex级别的简单索引 否则选择子集





































