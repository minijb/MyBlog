## 1. 折线图

series和DataFrame有一个plot属性，用于绘制基本图形。默认plot绘制的是折线图

```python
s = pd.Series(np.random.standard_normal(10).cumsum(), index=np.arange(0, 100, 10))
s.plot()
```

series中的索引会传入x轴，可以通过`use_index=False`来禁用，同样x轴的刻度可以通过xticks和xlim选项进行调整，y轴同理。大部分pandas的绘图设置可以接收ax用来放置子图

```python
df = pd.DataFrame(np.random.standard_normal((10, 4)).cumsum(0),
                  columns=["A", "B", "C", "D"],
                  index=np.arange(0, 100, 10))
plt.style.use('grayscale')
df.plot()
```

`df.plot()`等价于`df.plot.line()`

### series.plot参数列表

```python
label
ax
style
alpha	#不透明度
kind	#类型area,bar,barh,hist,kde,line,pie
logy	#在y轴使用对数缩放
use_index
rot		#刻度标签的旋转(0-360)
xticks
yticks
xlim
ylim
grid		#展示网格
```

### DataFrame.plot参数

```python
subplots  	#将每一列绘制在独立的子图中
sharex		#subplots=Ture则共享x轴，刻度和范围
sharey
figsize
title
legend
sort_columns
```

## 2. 柱状图

`plot.bar`和`plot.barh`可以绘制垂直和水平的图

```python
fig, axes = plt.subplots(2, 1)
data = pd.Series(np.random.uniform(size=16), index=list("abcdefghijklmnop"))
data.plot.bar(ax=axes[0], color="b", alpha=0.7)
#! figure,id=vis_bar_plot_ex,width=4.5in,title="Horizonal and vertical bar plot"
data.plot.barh(ax=axes[1], color="black", alpha=0.7)
```

在DataFrame中每一行的值分别分组并排到柱子的一组中

```python
df = pd.DataFrame(np.random.uniform(size=(6, 4)),
                  index=["one", "two", "three", "four", "five", "six"],
                  columns=pd.Index(["A", "B", "C", "D"], name="Genus"))
df
Out[3]: 
Genus         A         B         C         D
one    0.249985  0.402140  0.315799  0.592346
two    0.423345  0.308899  0.545124  0.527428
three  0.132928  0.656789  0.583622  0.643117
four   0.425841  0.795809  0.838373  0.477560
five   0.490895  0.299833  0.740065  0.766981
six    0.411601  0.878203  0.342906  0.429216


df.plot.bar()
```

注：列名称Genus被作为标题，我们可以传递stacked=True来书的每一行堆叠在一起

```python
df.plot.barh(stacked=True, alpha=0.5)
```

```python
tips = pd.read_csv("examples/tips.csv")
tips.head()

   total_bill   tip smoker  day    time  size
0       16.99  1.01     No  Sun  Dinner     2
1       10.34  1.66     No  Sun  Dinner     3
2       21.01  3.50     No  Sun  Dinner     3
3       23.68  3.31     No  Sun  Dinner     2
4       24.59  3.61     No  Sun  Dinner     4


party_counts = pd.crosstab(tips["day"], tips["size"])
party_counts = party_counts.reindex(index=["Thur", "Fri", "Sat", "Sun"])
party_counts
Out[5]: 
size  1   2   3   4  5  6
day                      
Thur  1  48   4   5  1  3
Fri   1  16   1   1  0  0
Sat   2  53  18  13  1  0
Sun   0  39  15  18  3  1

party_counts = party_counts.loc[:, 2:5]

party_pcts = party_counts.div(party_counts.sum(axis="columns"),
                              axis="index")
party_pcts
Out[6]: 
size         1         2         3         4         5         6
day                                                             
Thur  0.016129  0.774194  0.064516  0.080645  0.016129  0.048387
Fri   0.052632  0.842105  0.052632  0.052632  0.000000  0.000000
Sat   0.022989  0.609195  0.206897  0.149425  0.011494  0.000000
Sun   0.000000  0.513158  0.197368  0.236842  0.039474  0.013158

party_pcts.plot.bar(stacked=True)
```

> 其他详见 seaborn 和matlib

