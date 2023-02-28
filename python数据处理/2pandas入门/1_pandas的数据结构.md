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

DataFrame使矩阵的数据表，包含已排序的列集合，每一列可以使不同的值的类型(数值，字符串，布尔值)。DataFrame既有行索引也有列索引，可以被视为共享相同索引的Serises的字典

> 虽然DataFrame使二维的，但是我们可以通过分层方式展现更高维度的数据

有很多种方式可以构建DataFrame，常见的有包含等长数组或numpy数组的字典来进行初始化

```python
data = {
    'state': [ 'Ohio','Ohio','Ohio','Ohio'],
    'year' : [1,2,3,4],
    'pop' : [1.1,1.2,1.3,1.4]
} 
frame = pd.DataFrame(data)

frame
Out[7]: 
  state  year  pop
0  Ohio     1  1.1
1  Ohio     2  1.2
2  Ohio     3  1.3
3  Ohio     4  1.4
```

对于大型的DataFrame,`head`方法将只选出头部5行

```python
frame.head()
```

如果你指定了列的顺序，DataFrame的列将会按照指定顺序排列

```python
pd.DataFrame(data,columns=['year','pop','state'])
Out[9]: 
   year  pop state
0     1  1.1  Ohio
1     2  1.2  Ohio
2     3  1.3  Ohio
3     4  1.4  Ohio
```

如果列不包含在字典里，则显示NaN,当然我们也可以通过让左侧索引使用字符串

```python
pd.DataFrame(data,columns=['year','pop','state','None'],index = ['one','two','three','four'])
Out[10]: 
       year  pop state None
one       1  1.1  Ohio  NaN
two       2  1.2  Ohio  NaN
three     3  1.3  Ohio  NaN
four      4  1.4  Ohio  NaN
```

DataFrame的列，我们可以通过字典型标记或属性来检索成为一个Series

```python
frame2 = pd.DataFrame(data,columns=['year','pop','state','None'],index = ['one','two','three','four'])

frame2['state']
Out[13]: 
one      Ohio
two      Ohio
three    Ohio
four     Ohio
Name: state, dtype: object

frame2.year
Out[14]: 
one      1
two      2
three    3
four     4
Name: year, dtype: int64
```

> 注意：返回的Series与原DataFrame有相同的索引，且Seriesd的name也会被合理的设置

行的索引可以通过位置或特殊属性loc进行选取

```python
frame2.loc['three']
Out[17]: 
year        3
pop       1.3
state    Ohio
None      NaN
Name: three, dtype: object
```

列的引用可以修改，比如我们把None列赋值为标量或者数组

```python
frame2['None'] = 16.5
frame2
Out[20]: 
       year  pop state  None
one       2  1.1  Ohio  16.5
two       2  1.2  Ohio  16.5
three     3  1.3  Ohio  16.5
four      4  1.4  Ohio  16.5


frame2['None'] = np.arange(4.)
frame2
Out[24]: 
       year  pop state  None
one       2  1.1  Ohio   0.0
two       2  1.2  Ohio   1.0
three     3  1.3  Ohio   2.0
four      4  1.4  Ohio   3.0
```

> 注意赋值的长度必须匹配！！！

如果将Series赋值给一列的时候，Series的索引将会按照DataFrame的索引重新排序，并在空缺的地方补值

```python
val = pd.Series([-1.2,-1.5,1.7],index = ['two','four','one'])
frame2['None'] = val
frame2
Out[31]: 
       year  pop state  None
one       2  1.1  Ohio   1.7
two       2  1.2  Ohio  -1.2
three     3  1.3  Ohio   NaN
four      4  1.4  Ohio  -1.5
```

如果被赋值的列不存在，则会生成一个新的列。del关键字可以像删除字典那样删除列

```python
frame2['eee'] = frame2.year==2
frame2
Out[35]: 
       year  pop state  None    eee
one       2  1.1  Ohio   1.7   True
two       2  1.2  Ohio  -1.2   True
three     3  1.3  Ohio   NaN  False
four      4  1.4  Ohio  -1.5  False


del frame2['eee']
frame2.columns
Out[39]: Index(['year', 'pop', 'state', 'None'], dtype='object')
```

> 注：
>
> - frame2.eee无法创建新的列
> - 从DataFrame中选取的列是数据的视图，而不是拷贝，因此对Series的修改会映射到DataFrame中，如果需要拷贝需要显示的使用`copy`方法

还有一种数据形式是包含字典的嵌套字典

```python
pop = {
    'Nveada' : {2021:2.4,2022:2.9},
    'Ohio' : {2000:1.5,2001:1.7,2022:3.6}
}
frame3 = pd.DataFrame(pop)
frame3
Out[7]: 
      Nveada  Ohio
2021     2.4   NaN
2022     2.9   3.6
2000     NaN   1.5
2001     NaN   1.7
```

我们可以使用类似Numpy的方法进行转置

```python
frame3.T
Out[8]: 
        2021  2022  2000  2001
Nveada   2.4   2.9   NaN   NaN
Ohio     NaN   3.6   1.5   1.7
```

内部的键已经经过排序处理，如果显示的指明索引的话，内部字典的键将不会排序

```python
frame3 = pd.DataFrame(pop,index=[2001,2002,2003])
frame3
Out[10]: 
      Nveada  Ohio
2001     NaN   1.7
2002     NaN   NaN
2003     NaN   NaN
```

包含Series的字典将也可以用来构建DataFrame

```python
pdata = {
    'Ohio':frame3['Ohio'][:-1],
    'Nveada':frame3['Nveada'][:2]
}
pd.DataFrame(pdata)
Out[13]: 
      Ohio  Nveada
2001   1.7     NaN
2002   NaN     NaN
```

如果DataFrame的所有和列拥有name属性的话，name也会被显示

```py
frame3.index.name = 'year'
frame3.columns.name = 'state'
frame3
Out[16]: 
state  Nveada  Ohio
year               
2001      NaN   1.7
2002      NaN   NaN
2003      NaN   NaN
```

和Series类似，DataFrame的values属性会将包含在DataFrame中的数据以二维ndarray的形式返回

```python
frame3.values
Out[17]: 
array([[nan, 1.7],
       [nan, nan],
       [nan, nan]])
```

如果每列都有不同的dtype的话，则values的dtype会自动选择合适所有列的类型

```python
data = {
    'state': [ 'Ohio','Ohio','Ohio','Ohio'],
    'year' : [1,2,3,4],
    'pop' : [1.1,1.2,1.3,1.4]
} 
frame = pd.DataFrame(data)
frame
Out[19]: 
  state  year  pop
0  Ohio     1  1.1
1  Ohio     2  1.2
2  Ohio     3  1.3
3  Ohio     4  1.4
frame.values
Out[20]: 
array([['Ohio', 1, 1.1],
       ['Ohio', 2, 1.2],
       ['Ohio', 3, 1.3],
       ['Ohio', 4, 1.4]], dtype=object)
```

### 可以传入构造函数的有效输入

```python
2D ndarray
数组，列表，元组，字典
numpy结构化
Series构建的数组
字典和字典嵌套字典
字典或Series构成的列表
列表的列表(元组)
其他DataFrame
Numpy MaskedArray

```

## 3. 索引对象

pandas中的索引对象是用来存储周标签和其他元数据的。在构建Series和DataFrame睡觉哦，所用的任意数组或标签序列都可以在内部转化为索引对象

```python
obj = pd.Series(range(3),index = ['a','b','c'])
index = obj.index
index
Out[23]: Index(['a', 'b', 'c'], dtype='object')
index[1:]
Out[24]: Index(['b', 'c'], dtype='object')

```

索引对象是不可变的因此用户不可以修改索引对象

这使得多种数据结构中分享索引对象更加安全

```python
label = pd.Index(np.arange(3))
obj2 = pd.Series([-1.5,-2.5,0],index = label)
obj2
Out[29]: 
0   -1.5
1   -2.5
2    0.0
dtype: float64
obj2.index is label
Out[30]: True
```

同时索引对象也是一个固定大小的集合

```python
frame3
Out[31]: 
state  Nveada  Ohio
year               
2001      NaN   1.7
2002      NaN   NaN
2003      NaN   NaN
'Ohio' in frame3
Out[32]: True
'Ohio' in frame3.columns
Out[33]: True
2003 in frame3.index
Out[34]: True

```

和python集合不同，pandas索引对象可以包含重复标签

```python
dup_labels = pd.Index(['foo','foo','bar','bar'])
dup_labels
Out[36]: Index(['foo', 'foo', 'bar', 'bar'], dtype='object')
```

根据重复标签进行筛选会选取重复标签对应的数据

### 索引对象的方法和属性

```python
append #二外的索引粘贴到原索引后，产生一个新索引
difference 	#计算两个索引的差集
intersection	#计算两个索引的交集
union	#计算并集
isin	#计算每一个值是否在传值容器中的布尔数组
delete	#将位置i的元素删除，并产生新的索引
drop	#根据传参输出指定的索引值，并产生新的索引
insert	#在位置i插入元素，并产生新的索引
is_monotonic	#入股索引序列递增，返回True
is_unique	#如果索引序列唯一返回True
unique		#计算索引的唯一值序列
```

