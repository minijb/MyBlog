绘制简单的图像

```python
data = np.arange(10)
data
Out[3]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
plt.plot(data)
```

## 1. 图片和子图

matplotlib绘制的图片位于Figure中，可以使用`plt.figure`生成一个新的图片

```python
fig = plt.figure()
```

figure总有一些选项比如figsize来确保图片的大小。

你不能使用空白的图片进行绘画。你需要使用`add_subplot`创建一个或者多个子图(subplot)

```python
ax1 = fig.add_subplot(2, 2, 1)
```

这个代码的意思是创建一个`2*2`的图形，并且我们选择了四边形中的第一个

```python
ax2 = fig.add_subplot(2, 2, 2)

ax3 = fig.add_subplot(2, 2, 3)
plt
```

此时我们可以看见3中空白的子图。

此时运行一下语句

```python
plt.plot(np.random.randn(50).cumsum(),'k--')
#效果相同
ax3.plot(np.random.standard_normal(50).cumsum(), color="black",
         linestyle="dashed")
```

'k--'用于绘制黑色分段线style选项。`fig.add_subplot`返回的是AxesSubPlot对象，使用该对象你可以直接在其他空白子图上调用对象的实例方法进行绘图

```python
ax1.hist(np.random.standard_normal(100), bins=20, color="black", alpha=0.3)
ax2.scatter(np.arange(30), np.arange(30) + 3 * np.random.standard_normal(30))
```

使用子网格创建图片是十分常见的内容，有一个便捷方法，它创造了新的子图，并返回包含生成子图对象的numpy数组

```python
fig, axes = plt.subplots(2, 3)
axes
array([[<AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>],
       [<AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>]], dtype=object)
```

我们可以直接通过索引的方式选中子图并进行绘画`axes[0,1]`,sharex,sharey表明子图分别拥有相同的x轴或者y轴。当你在相同的比例下进行数据对比时，sharex和sharey会非常有用

#### 参数列表

```python
nrows
ycols
sharex		#调整xlim
sharey
subplot_kw 	#add_subplot的关键字参数字典，用于生成子图
**fig_kw		#额外关键字
```

### 1.1 调整子图之间的距离

matplotlib会在子图的外部和子图之间留出一定的距离，这个距离相对于图的高宽来指定的，所以通过GUI窗口来调整的大小，那么图就会自动调整。你可以使用图对象上的subplots_adjust来更改间距，也可以用作顶层函数

```python
subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)
```

`wspace`和`hspace`分别控制的是图片的宽度和高度百分比，以用作子图间的间距。

我们可以将图的间距缩小到0

```python
fig, axes = plt.subplots(2, 2, sharex=True, sharey=True)
for i in range(2):
    for j in range(2):
        axes[i, j].hist(np.random.standard_normal(500), bins=50,
                        color="black", alpha=0.5)
fig.subplots_adjust(wspace=0, hspace=0)
```

## 2. 颜色，标记和线类型

可以通过可选字符串来指定颜色和类型

```python
ax.plot(x,y,'g--')
```

这种在字符串中指定颜色和线条。

```python
ax.plot(x,y,linestyle='--',color='g')
```

有很多颜色缩写被用于常用颜色，也可以直接指定十六进制颜色代码的方式来指定任何颜色。

在折线图中还可以有标记来凸显实际的数据点，注：先类型，标记类型必须在颜色后面

```python
plt.plot(np.random.randn(30).cumsum(),'ko--')
```

如下和上面等价

```python
ax.plot(np.random.standard_normal(30).cumsum(), color="black",
        linestyle="dashed", marker="o");
```

对于折线图，你会注意到后继的点默认是线性内插的，可以通过drawstyle选项进行更改

```python
ax.plot(data, color="black", linestyle="dashed", label="Default");
fig
ax.plot(data, color="black", linestyle="dashed",
        drawstyle="steps-post", label="steps-post");
fig
ax.legend()
```

由于我们传递了label，我们可以使用plt.legend为每一条线生成一个用于区分的图例

## 3.刻度，标签和图例

pyplot包含了像xlim，xticks和xtickslabels等方法，这些方法分别控制了绘图范围，刻度位置以及刻度标签。

使用方式：

- 在没有函数参数的情况下调用，返回当前的参数值，
- 传入参数的情况下调用，并设置参数值，会改变

所有方法如果都会在当前活动的或最近的AxesSubPlot上生效，这些方法对应子途中的两个方法

- xlim对应`ax.get_lim和ax.set_lim`

我们更倾向于使用子图的方法

### 3.1 这只标题，轴标签，刻度和刻度标签

```python
fig, ax = plt.subplots()
ax.plot(np.random.standard_normal(1000).cumsum());
```

设置刻度很简单

```python
ticks = ax.set_xticks([0, 250, 500, 750, 1000])
labels = ax.set_xticklabels(["one", "two", "three", "four", "five"],
                            rotation=30, fontsize=8)
ax.set_xlabel("Stages")
ax.set_title("My first matplotlib plot")
fig
```

设置y轴标签是相同过程，x替换成y就可以了。轴类型拥有一个set方法，允许批量设置绘图属性，

```python
fig
#%%
props = {
    'title' : 'My first matplotlib',
    'xlabel' : 'stages'
}
ax.set(**props)
```

### 3.2 添加图例

```python
fig, ax = plt.subplots()
ax.plot(np.random.randn(1000).cumsum(), color="black", label="one");
ax.plot(np.random.randn(1000).cumsum(), color="black", linestyle="dashed",
        label="two");
ax.plot(np.random.randn(1000).cumsum(), color="black", linestyle="dotted",
        label="three");
```

可以使用`ax.legend`或`plt.legend`自动生成图例

```python
ax.legend()
fig
```

legend有loc参数，可以用来设置位置。(ax.legend?查看详细信息)

### 3.3 注释与子图加工

添加注释的种类：文本，箭头，其他图形 ，对应text,arrow和annote方法来添加注释和文本，

```python
ax.text(x,y,'xxxx',family = 'xxx', fontsize=xx)
```

```python
from datetime import datetime

fig, ax = plt.subplots()

data = pd.read_csv("examples/spx.csv", index_col=0, parse_dates=True)
spx = data["SPX"]

spx.plot(ax=ax, color="black")

crisis_data = [
    (datetime(2007, 10, 11), "Peak of bull market"),
    (datetime(2008, 3, 12), "Bear Stearns Fails"),
    (datetime(2008, 9, 15), "Lehman Bankruptcy")
]

for date, label in crisis_data:
    ax.annotate(label, xy=(date, spx.asof(date) + 75),
                xytext=(date, spx.asof(date) + 225),
                arrowprops=dict(facecolor="black", headwidth=4, width=2,
                                headlength=4),
                horizontalalignment="left", verticalalignment="top")

# Zoom in on 2007-2010
ax.set_xlim(["1/1/2007", "1/1/2011"])
ax.set_ylim([600, 1800])

ax.set_title("Important dates in the 2008-2009 financial crisis")
```

我们通过`ax.annotate`在指定的位置绘制标签。通过set_xlim,set_ylim设置x，y轴。绘制图形可以使用

```python
fig, ax = plt.subplots(figsize=(12, 6))
rect = plt.Rectangle((0.2, 0.75), 0.4, 0.15, color="black", alpha=0.3)
circ = plt.Circle((0.7, 0.2), 0.15, color="blue", alpha=0.3)
pgon = plt.Polygon([[0.15, 0.15], [0.35, 0.4], [0.2, 0.6]],
                   color="green", alpha=0.5)
ax.add_patch(rect)
ax.add_patch(circ)
#! figure,id=vis_patch_ex,width=4in,title="Data visualization composed from three different patches"
ax.add_patch(pgon)
```

## 4. 保存到文件

```python
plt.savefig('xxxx',dpi=xxxx,bbox_inches='tight')
```

可以设置dpi以及修建实际图形的空白

写入内存，之后再处理

```python
from io import BytesIO
buffer = BytesIO
plt.saveflg(buffer)
plot_data = buffer.getvalue()
```

## 5. matplotlib设置

可以直接再顶层设置画布的属性(rc)

```python
plt.rc('figure',figsize=(10,10))
```

rc的第一个参数想要自定义设置的组件比如`figure  axes  xtick ytick grid legend`

```python
font_options= {'family':'monospace','weight':'bold','size':'small'}
plt.rc('font',**font_options)
```

