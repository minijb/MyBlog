## 1. 为什么提出CNN

- 全连接的参数过多，CNN可以减少参数

- 影像的特性，我们只需要部分的特征

<img src="https://s2.loli.net/2022/10/22/w6CQu8P3GVJYkXA.png" alt="image.png" style="zoom:33%;" />

<font color="red" size=4>第一个优化</font>

receptive field，只关注field内在的特征

<img src="https://s2.loli.net/2022/10/22/3E9kzueZmhNt2Gr.png" alt="image.png" style="zoom:33%;" />

每一个field可以是重叠的

<img src="https://s2.loli.net/2022/10/22/fZ7arsgR6kMzy8F.png" alt="image.png" style="zoom:33%;" />

<font color="blue">出现的一些问题</font>

- <font color="blue"> 不同的neurons是否可以含有不同的size的field</font>
- <font color="blue">是否有些neurons只考虑一个channels</font>

**Typical Setting**

- field考虑所有的channel
- 可以直接通过kernel size来表示

- 为了收集信息可以设置一个stride，每次移动一个stride就可以获得一个新的kernel
- 为例收集边角的信息，我们可以使用padding来增加一个白边

<img src="https://s2.loli.net/2022/10/22/P5Mcj1Z6e2O8uEN.png" alt="image.png" style="zoom:33%;" />

<font color="red" size=4>第二个优化</font>

比如有一个neuron是用来检测鸟嘴的，一张图片中，可能就只有一个鸟嘴，那么我们需要将每一个field里面都设置一个neuron用来侦测鸟嘴吗？？？不同size 的field需要单独设置一个neuron吗

<img src="https://s2.loli.net/2022/10/22/E9sBU8gwDA76keH.png" alt="image.png" style="zoom:33%;" />

我们能否让不同field的neuron共享参数？

<img src="https://s2.loli.net/2022/10/22/QSFpt4vDRiCBu3k.png" alt="image.png" style="zoom:33%;" />

**Typical Setting**

- each receptive field has a set of neurons
- each receptive field has the neurons with the same set of parameters

<img src="https://s2.loli.net/2022/10/22/WKJ5Y9LCphVq7Hw.png" alt="image.png" style="zoom:33%;" />

<font color="red" size=4>总结</font>

<img src="https://s2.loli.net/2022/10/22/tUcqyVKDHeJEIAW.png" alt="image.png" style="zoom:33%;" />

> 另一种解释
>
> 见https://www.bilibili.com/video/BV1Wv411h7kN?p=31&vd_source=8beb74be6b19124f110600d2ce0f3957
>
> - filter的高度: 当前层的高度和输入的高度相同！！！！
>
> - 每经过一层， 感受野就会变大
> - 那个共用的参数就是filter内的数字

 <font color="red" size=4>第一个优化：pooling</font>

subsampling the pixels will not change the object

<img src="https://s2.loli.net/2022/10/22/dE8nHIOxThG5izS.png" alt="image.png" style="zoom: 33%;" />

**不同的pooling**

- max pooling

<img src="https://s2.loli.net/2022/10/22/WBR3JrtxpG9OmCl.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/22/8axVfB7iChnupdL.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/22/j7XV5EwDxMAoFRI.png" alt="image.png" style="zoom:33%;" />

<font color="blue">如果我们不希望丢失细小特征可以不适用pooling</font>

**flatten操作**

将矩阵拉直称为一个向量

<img src="https://s2.loli.net/2022/10/22/5s1Jj8WUKLVnySM.png" alt="image.png" style="zoom:33%;" />

> CNN可以用来下围棋，因为围棋和图像有着相同的特性
>
> 各个特征可以出现再棋盘的不同位置
>
> 但是pooling不能出现

<font color="blue">现在的问题就是，同一个图像，如果不同的大小，cnn无法很好的处理</font>

- cnn is not invariant  to scaling and rotation

> 1*1卷积的作用
>
> - 降维/升维：根据1*1卷积核的个数决定的
> - 跨通道信息交融
> - 较少参数量：先降维，然后再实行原本的卷积
> - 怎么模型深度，提高非线性表示能力
>
> 

