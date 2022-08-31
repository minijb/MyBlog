pandas的两个常用的工具数据结构,*Series*和*DataFrame*

## 1. Series

**series**是一种一维的数组型对象，包含一个值序列(与numpy中的类型相似)，并包含数据标签，称为索引(index)。

```python
import pandas as pd
obj =  pd.Series([4,5,-5,3])
obj
Out[2]: 
0    4
1    5
2   -5
3    3
dtype: int64
```

索引的长度默认为[0,n-1],我们可以通过`values`和`index`属性分别获得值和索引

```python
obj.values
Out[3]: array([ 4,  5, -5,  3], dtype=int64)
obj.index
Out[4]: RangeIndex(start=0, stop=4, step=1)
```

我们也可以建立一个索引序列，用标签标识每一个数据点

```python
obj2 = pd.Series([4,5,-5,3],index=['d','b','a','c'])
obj2
Out[6]: 
d    4
b    5
a   -5
c    3
dtype: int64

obj2.index
Out[7]: Index(['d', 'b', 'a', 'c'], dtype='object')
```

我们可以通过标签来进行数据索引

```python
obj2['d']=100
obj2[['c','d','a']]
Out[11]: 
c      3
d    100
a     -5
dtype: int64
```

在Series中可以使用numpy风格的函数和操作，比如通过布尔数组进行过滤操作，与标量相乘，或是应用数学函数

```python
obj2[obj2>0]
Out[12]: 
d    100
b      5
c      3
dtype: int64

obj2*2
Out[13]: 
d    200
b     10
a    -10
c      6
dtype: int64

np.exp(obj2)
Out[16]: 
d    2.688117e+43
b    1.484132e+02
a    6.737947e-03
c    2.008554e+01
dtype: float64
```

从另一个角度考虑Series，可以认为它使一个长度固定且有序的字典，因为它将所有制和数据按位置配对。在可以使用字典的上下文中，我们也可以使用Series

```python
'b' in obj2
Out[17]: True
'e' in obj2
Out[18]: False

```

如果我们已经有数据在python字典中，我们可以将它转化为一个Series

```python
sdata = {
    'Ohio':3500,
    'Taxas':70000,
    'Oregon' : 16000,
    'Utah' : 5000
}
obj3 = pd.Series(sdata)
obj3
Out[23]: 
Ohio       3500
Taxas     70000
Oregon    16000
Utah       5000
dtype: int64
```

当使用字典初始化Series时，索引僵尸排序好的字典建，我们可以将字典建按照我们想要的顺序传递给函数，从何使索引的顺序符合要求

```python
states = ['Taxas','Oregon','Utah','Ohio','Ca']
obj4 = pd.Series(sdata,index = states)
obj4
Out[29]: 
Taxas     70000.0
Oregon    16000.0
Utah       5000.0
Ohio       3500.0
Ca            NaN
dtype: float64
```

因为Ca没有出现在字典中，所以它对应的值就是NaN，这是pandas处理缺失值或者NA值的方式

pandas使用`isnull`和`notnull`函数来检查确实数据

```python
pd.isnull(obj4)
Out[30]: 
Taxas     False
Oregon    False
Utah      False
Ohio      False
Ca         True
dtype: bool

pd.notnull(obj4)
Out[31]: 
Taxas      True
Oregon     True
Utah       True
Ohio       True
Ca        False
dtype: bool
```

同时，两个方法也是Series的实例方法

```python
obj4.isnull()
Out[32]: 
Taxas     False
Oregon    False
Utah      False
Ohio      False
Ca         True
dtype: bool
```

在很多应用中，自动对齐索引是一个非常有用的特性

```python
obj3
Out[33]: 
Ohio       3500
Taxas     70000
Oregon    16000
Utah       5000
dtype: int64
obj4
Out[34]: 
Taxas     70000.0
Oregon    16000.0
Utah       5000.0
Ohio       3500.0
Ca            NaN
dtype: float64
obj3+obj4
Out[35]: 
Ca             NaN
Ohio        7000.0
Oregon     32000.0
Taxas     140000.0
Utah       10000.0
dtype: float64
```

Series对象自身和索引都有name属性，这个特征与pandas其他重要功能集成在一起

```python
obj4.name = 'population'
obj4.index.name = 'state'
obj4
Out[40]: 
state
Taxas     70000.0
Oregon    16000.0
Utah       5000.0
Ohio       3500.0
Ca            NaN
Name: population, dtype: float64
```

Series的索引可以通过按位置赋值的方式进行改变

```python
obj
Out[41]: 
0    4
1    5
2   -5
3    3
dtype: int64
obj.index=['Bob','Jack','Man','Tim']
obj
Out[43]: 
Bob     4
Jack    5
Man    -5
Tim     3
dtype: int64
```

## 2. DataFrame

