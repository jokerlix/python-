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


#### 丢弃指定轴上的项

``` python

In [3]: obj = pd.Series(np.arange(5.0),index = ['a','b','c','d','e'])               

In [4]: obj                                                                         
Out[4]: 
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64

In [5]: new_obj = obj.drop('c')                                                     

In [6]: new_obj                                                                     
Out[6]: 
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64

In [7]: obj.drop(['d','c'])                                                         
Out[7]: 
a    0.0
b    1.0
e    4.0
dtype: float64
```
`drop` 删除指定轴上的内容

对于 `DataFrame` 可以删除任意轴上的索引值
``` python
In [8]: data = pd.DataFrame(np.arange(16).reshape(4,4),index = ['Ohio','Colorado','U
   ...: tah','New York'],columns=['one','two','three','four'])                      

In [9]: data                                                                        
Out[9]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [10]: data.drop(['Colorado','Ohio'])                                             
Out[10]: 
          one  two  three  four
Utah        8    9     10    11
New York   12   13     14    15

In [11]: data.drop('two',axis=1)                                                    
Out[11]: 
          one  three  four
Ohio        0      2     3
Colorado    4      6     7
Utah        8     10    11
New York   12     14    15

In [12]: data                                                                       
Out[12]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [13]: data.drop('two')                       
```
#### 其中drop()方法不能只写一个列名，需要加上axis='columns'/axis=1
``` python
In [17]: obj                                                                        
Out[17]: 
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64

In [18]: obj.drop('c',inplace=True)                                                 

In [19]: obj                                                                        
Out[19]: 
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64

```
`inplace` 会销毁所有被删除的数据

#### 索引、选取、过滤

>> Series 索引的索引值不只是整数
``` python
In [20]: obj = pd.Series(np.arange(4.0),index = ['a','b','c','d'])                  

In [21]: obj                                                                        
Out[21]: 
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64
In [23]: obj['b']                                                                   
Out[23]: 1.0
In [25]: obj[3]                                                                     
Out[25]: 3.0

In [26]: obj[2:3]                                                                   
Out[26]: 
c    2.0
dtype: float64
In [28]: x = np.array([[1,2,3],[4,5,6],[7,8,9]])                                    

In [29]: x                                                                          
Out[29]: 
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])

In [30]: x[1]                                                                       
Out[30]: array([4, 5, 6])

In [31]: x[2]                                                                       
Out[31]: array([7, 8, 9])

```
##### 切片为前闭后开

区别 `[1,3]` 与 `[1:3]`

##### 利用标签的切片是前闭后开的

``` python
In [42]: obj                                                                        
Out[42]: 
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64

In [43]: obj['a':'c']                                                               
Out[43]: 
a    0.0
b    1.0
c    2.0
dtype: float64

```
#### loc和iloc进行选取

<br>`loc` 使用轴索引 
<br> `iloc` 使用整数索引

``` python
In [70]: data                                                                       
Out[70]: 
    a   b   c   d
A   0   1   2   3
B   4   5   6   7
C   8   9  10  11
D  12  13  14  15
In [69]: data.iloc[2]                                                               
Out[69]: 
a     8
b     9
c    10
d    11
Name: C, dtype: int64
In [68]: data.loc['A']                                                              
Out[68]: 
a    0
b    1
c    2
d    3
Name: A, dtype: int64
```

#### 以上两个索引函数适用于多个标签或者一个标签的切片

``` python
In [80]: data                                                                       
Out[80]: 
    a   b   c   d
A   0   1   2   3
B   4   5   6   7
C   8   9  10  11
D  12  13  14  15

In [81]: data.loc[:'C','a']                                                         
Out[81]: 
A    0
B    4
C    8
Name: a, dtype: int64
```
#### 前面为选取行 后面选取列 最后的[]条件选取行

`data.iloc[:, :3][data.d > 5]`

```
In [89]: data.iloc[:, :3][data.d > 5]                                               
Out[89]: 
    a   b   c
B   4   5   6
C   8   9  10
D  12  13  14

```
>> df[val]                选取单列
>> df.loc[val]            选取单行
>> df.loc[:,val]          通过标签，选取单列或者列子集
>> df.loc[val1,val2]      通过标签，同时选取行和列
>> df.iloc[where]         通过整数位置 选取行
>> df.iloc[:,where]       通过整数位置 选取当个行
>> df.iloc[where_i,where_j]通过整数位置同时选取行和列
>> df.at[label_i,label_j] 通过行和列标签选取单个变量
>> df.iat[i,j]            通过行和列的位置 选取单个变量

#### 整数索引容易出现问题，非整数索引不会出现问题

``` python
In [101]: ser                                                                       
Out[101]: 
0    0
1    1
2    2
dtype: int64

In [102]: ser[-1]                                                                   
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-inpu

In [103]: ser = pd.Series(np.arange(3),index=['a','b','c'])                         

In [104]: ser                                                                       
Out[104]: 
a    0
b    1
c    2
dtype: int64

In [105]: ser[-1]                                                                   
Out[105]: 2

```
#### 如果轴索引含有整数 数据选取总会使用标签 为了选取准确 必须使用loc iloc 进行选取

#### 算术运算和数据对齐
<br> pandas 可以对不同索引对象进行算术运算 
<br> 将对象相加时，存在不同的索引对 结果的索引是索引对的并集
``` python
In [106]: s1 = pd.Series([7.3,-2.5,3.4,1.5],index = ['a','c','d','e'])              

In [107]: s2 = pd.Series([-2.1,3.6,-1.5,4,3.1],index = ['a','c','e','f','g'])       

In [108]: s1                                                                        
Out[108]: 
a    7.3
c   -2.5
d    3.4
e    1.5
dtype: float64

In [109]: s2                                                                        
Out[109]: 
a   -2.1
c    3.6
e   -1.5
f    4.0
g    3.1
dtype: float64

In [110]: s1 + s2                                                                   
Out[110]: 
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN

```
在不重叠处引入NA值

<br>

##### 如果DataFrame对象相加,没有共用的列或行标签,结果都会是空

``` python
In [132]: df                                                                        
Out[132]: 
   a  b   c   d
0  0  1   2   3
1  4  5   6   7
2  8  9  10  11

In [133]: df2                                                                       
Out[133]: 
    a     b   c   d   e
0   0   1.0   2   3   4
1   5   NaN   7   8   9
2  10  11.0  12  13  14
3  15  16.0  17  18  19

In [134]: df + df2                                                                  
Out[134]: 
      a     b     c     d   e
0   0.0   2.0   4.0   6.0 NaN
1   9.0   NaN  13.0  15.0 NaN
2  18.0  20.0  22.0  24.0 NaN
3   NaN   NaN   NaN   NaN NaN

In [135]: df.add(df2,fill_value=0)                                                  
Out[135]: 
      a     b     c     d     e
0   0.0   2.0   4.0   6.0   4.0
1   9.0   5.0  13.0  15.0   9.0
2  18.0  20.0  22.0  24.0  14.0
3  15.0  16.0  17.0  18.0  19.0

```



















