## 1. Gragh Embeding的作用

如果有10个节点，我们一般想到的是使用10维度的one-hot向量

缺点：

- 如果node数量的大那么one-hot的存储就会很大
- 丢失了图的信息

## 2. DeepWalk

 Deepwalk是一种将随机游走(random walk)和word2vec两种算法相结合的图结构数据挖掘算法。该算法能够学习网络的隐藏信息，能够将图中的节点表示为一个包含潜在信息的向量.

​     该算法主要分为随机游走和生成表示向量两个部分。首先利用随机游走算法(Random walk)从图中提取一些顶点序列；然后借助自然语言处理的思路，将生成的定点序列看作由单词组成的句子，所有的序列可以看作一个大的语料库(corpus)，最有利用自然语言处理工具word2vec将每一个顶点表示为一个维度为d的向量。

​	通过随机游走得到一个序列

![image.png](https://s2.loli.net/2022/10/12/ZRDcArlk5QMJwP1.png)

之后通过word-embeding的方式训练这个序列

https://zhuanlan.zhihu.com/p/45167021

## 3. Line算法

deepwalk主要运用在无向图中，Line算法都可以---在大规模图上表示节点之间的结构信息

- 一阶：局部的结构信息（如果节点之间边的weight比较大，那么两者应该是相似的）
- 二阶：节点的邻居，共享邻居的节点可能是相似的

https://blog.csdn.net/weixin_38877987/article/details/118422847

Line算法：在度比较低的节点上效果不好

最终的向量就是一阶和二阶向量的相加

## 3. node2vec

一些概念

- homophily:同质性-----社区关系结构，即同一社区节点表示相似
- structural equivalence-----拥有类似结构特征的节点表示相似

![image.png](https://s2.loli.net/2022/10/12/k5g7B3MrA2wzTXn.png)

有策略的随机游走

![image.png](https://s2.loli.net/2022/10/13/vrZSAJwUgNLQqcT.png)

![image.png](https://s2.loli.net/2022/10/13/VdNFoqGDzS8l2tw.png)

![image.png](https://s2.loli.net/2022/10/13/BhfbowspxIGKdcz.png)

### 边的embeding

![image.png](https://s2.loli.net/2022/10/13/mEWVZX8r73fKB6I.png)

## 4. struc2vec

之前的算法都是基于近邻关系的，但是有些节点没有近邻但是也有相似的结构，如图

![image.png](https://s2.loli.net/2022/10/13/LabrdHYmFAXcZ6R.png)

一些定义

![image.png](https://s2.loli.net/2022/10/14/Xl5kgJ4CNzFyL6r.png)

![image.png](https://s2.loli.net/2022/10/14/aYXEQKdyIm8vWuZ.png)

![image.png](https://s2.loli.net/2022/10/14/zpAu9B3d67SsqCm.png)

![image.png](https://s2.loli.net/2022/10/14/HUxAtpLdfJZRMYv.png)

## 5. SDNE

之前都是使用了浅层的结构，这里使用了多层千层模型![image.png](https://s2.loli.net/2022/10/14/pLw9WENzh3GntPd.png)

## 6. 代码

https://github.com/shenweichen/GraphEmbedding

