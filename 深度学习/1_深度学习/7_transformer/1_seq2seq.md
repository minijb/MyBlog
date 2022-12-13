# 1. 基础

seq2seq模型主要有两部分组成：encoder，decoder

<img src="https://s2.loli.net/2022/10/26/fzhuWBRswjMqm49.png" alt="image.png" style="zoom:50%;" />

### 1.1 encoder

encoder的作用是输入一排向量，输出一排向量，现在最经典的就是transformer

<img src="https://s2.loli.net/2022/10/26/2AvGWnYMhDi1l3x.png" alt="image.png" style="zoom:33%;" />

### 1.2 transformer架构

<img src="https://s2.loli.net/2022/10/26/j1VIMyhRDoAfLEF.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/26/5KfNHGmZXlYQsug.png" alt="image.png" style="zoom: 33%;" />

## 2.1 decoder

步骤：自动产生语句

- 在输入上打上标签（begin）
- 输出的vector的长度就是vocabulary表的length，一般在之前都会有一个softmax得到每个单词的概率，然后我们只需要概率最大的哪个输出值
- 拿之前的结果作为输入，得到新的输出以此类推



有一个小问题：就是使用自己产生的结果最为输入，如果之前的结果有错误，会不会造成误差传播问题

<img src="https://s2.loli.net/2022/10/27/81ZSnKLqzT4CMsi.png" alt="image.png" style="zoom:33%;" />

**transformer中的decoder结构**

<img src="https://s2.loli.net/2022/10/27/Vu9ZniHQSNXdq1l.png" alt="image.png" style="zoom:33%;" />

**encoder和decoder的差异**

- 多了最后的softmax
- 多了中间一块
- 第一块中多了一个masked

<img src="https://s2.loli.net/2022/10/27/Qg3GshSfwm9eBUd.png" alt="image.png" style="zoom: 50%;" />

**masked self-attention**

因为特征提取的时候，我们是需要查看上下文的，但是产生新的句子的时候我们*只需要考虑当前位置以及之前位置的情况就可以*

<img src="https://s2.loli.net/2022/10/27/GSEdCYxjbUNHqZz.png" alt="image.png" style="zoom:33%;" />

**如何自主决定输出的长度**

我们需要在词向量表中添加一个end符号用来中断，decoder可以输出end符号，一旦输出end符号，整个输出就直接中断

<img src="https://s2.loli.net/2022/10/27/zTKX8pavmYZ2BeN.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/27/BhRf8g9FNeGAUXd.png" alt="image.png" style="zoom:33%;" />

## 3. Non-atuoregressive decoder

以中文句子产生为例，如果我们需要产生一句话，AT就和上文中的一样，而nat则每次输入都是begin向量，直接产生vector，

**如何解决产生的长度问题？**

- 训练一个classifier和encoder并行，产生句子的长度
- 直接输出一个长句子，忽视end之后的结果

**优势**

- 平行产生结果
- 在输出长度控制中效果比较好

**劣势**

- 结果一般比at差

<img src="https://s2.loli.net/2022/10/27/KGZfkESrAigDI9p.png" alt="image.png" style="zoom:33%;" />

## 4. cross attention

<img src="https://s2.loli.net/2022/10/27/ZScBkMRrzlG1LdJ.png" alt="image.png" style="zoom: 50%;" />

<img src="https://s2.loli.net/2022/10/27/LCMEfWJuqg5bDZX.png" alt="image.png" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/10/27/TAgVBv4Hk2chIOP.png" alt="image.png" style="zoom:33%;" />

**不同的crossattention**

<img src="https://s2.loli.net/2022/10/27/bPBQyzRscU8vWOZ.png" alt="image.png" style="zoom: 50%;" />

## 5. train

训练其实就是让模型输出的东西和实际的接近，这里我们输出的是每一个word的概率，实际是一个onhot图，看起来就像一个分类问题

<img src="https://s2.loli.net/2022/10/28/AmPkioFOeWL6ztN.png" alt="image.png" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/10/28/PI7L6vrBODaMkdX.png" alt="image.png" style="zoom: 50%;" />

同时我们需要注意的是：不仅仅是word，我们还需要将begin和end也同样训练出来

**teacher forceing**

在decoder训练的时候，我们输入的ground truth

**copy mechanism**

就比如我们有一句话，此时如下

<img src="https://s2.loli.net/2022/10/28/mRnrN45PUDEAFBi.png" alt="image.png" style="zoom:33%;" />

机器看我我们的名字等陌生内容，不能很好的产生单词，但是如果我们只需要训练它如何进行**复制**就比较好了

**Guided attention**

有些很明显的错误：如果我们只有长例子，没有短例子，那么在短句子就可能出问题。Guided attention就是强制attention有一个固定的样貌。

<img src="https://s2.loli.net/2022/10/28/obvDeMGpETI2gAW.png" alt="image.png" style="zoom:33%;" />

**Beam Search**

