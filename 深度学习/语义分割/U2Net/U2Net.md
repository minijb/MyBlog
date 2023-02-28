## 1. U2Net的优点

1. 有不同大小的感受野，可以获得更多的乡下问信息
2. 使用了池化操作，增加了深度，但是没有增加参数的数量---不同使用图像分类的骨干网络---最后直接使用的是1*1的卷积进行二分类的

> 补充：U2Net原本的任务是SOD显著目标检测，也就是二分类的图像分割---更适合作为异常分析！！！

## 2. Introduction

传统网络，提取语义信息的特征，而不是局部细节和全局的对比信息(后者在分割领域很重要)，<font color="red">也就是说传统的网络结构不适合做分割</font>，同时使用imagenet网络做预训练的话也是效率低下的，如果目标数据和imagenet的关注点是不一样的。

<font color="red">也就是说，U2Net是不需要进行预训练的，同时和传统的网络架构是有差别的</font>

当前的SOD网路的缺点

- 过于复杂，添加了很多特征聚合模块
- 经常牺牲特征图的高分辨率来实现更深层次的网络架构(resnet 缩小到1/4)
- 相对的，高分辨率在分割领域很重要！！！<font color="red">因此本model实现了在深层次中，维持高分辨率的特征图</font>

U2Net就是解决了以上的缺点

- 使用了嵌套的U形结构
- 在底层：通过**RSU**来收集多尺度的特征，而不用降低特征图的分辨率，<font color="red">RSU</font>
- 在顶层：是一个U型结构

## 3. 残差U型结构

- 传统的神经网络如VGG和resnet通常使用小的卷积核，因为太小了，在浅层提取全局的特征
- 这边文章提到了一个直接夸大感受野的方法
  - Deeplab: Semantic image segmentation with deep convolutional nets
  - A bi-directional message passing model for salient object detection.（作者由此提出基于各个层次特征图加权融合的模型并加入了双向信息传递，从高层语义到底层边缘信息，以及反向传递）
  - **这两种方法的参数量和运算速度不行**
- PoolNet [22] 从金字塔池化模块 (PPM) [57] 调整并行配置，该模块在降采样的特征图上使用小型内核过滤器，而不是原始大小特征图上的扩张卷积。但是通过直接上采样和级联 (或加法) 来融合不同规模的特征可能会导致高分辨率特征的退化。

![poolnet](https://img-blog.csdnimg.cn/20200731150738267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjMzNDU1,size_16,color_FFFFFF,t_70#pic_center)

- 提出了RSU---新的残差模块

![image.png](https://s2.loli.net/2022/11/04/cHhN8CY9EW4IemD.png)

主要有三部分

1. 一层普通卷积，$F_1(x)$---用来将特征层的层数从$C_{in}$转化为$C_{out}$
2. 第二部分是：类UNet的encoder和decoder层，高度为L，L越大，代表着更深的UNet接口，也就是更多的pooling操作，更大的感受野，以及更多的本地和全局的信息。
   - 多尺度信息是通过逐级下采样获得的，并通过上采样，级联，卷积恢复到高分辨率
   - 逐级上采样而不是直接上采样，可以答复减少惊喜的特征损失
3. 最后还有个残差结构，直接和最开始的$C_{out}$进行求和

![image.png](https://s2.loli.net/2022/11/05/9ywWIof3FUGpKLm.png)

**作者描述自己结构和resnet的区别**

- 使用类似Unet的结构，来代替中间的单模块的卷积层

<img src="https://s2.loli.net/2022/11/04/cHhN8CY9EW4IemD.png" alt="image.png" style="zoom: 80%;" />

- 使用本地的特征代替，original feature

> 个人感觉，这里主要描述的是原图中addition部分的作用，如果可以像他这样的话，可以不可以直接向resnet一样继续叠加，或者增加中间层！！

这种设计直接从每个残差块从多个尺度中提取特征。U型模块的计算量是很小的

> 对比了dense block (DSE)，inception block (INC)，plain convolution (PLN) and residual block (RES) blocks

## 4. U2net的结构

不同的U2Net结构：stacked hourgalss network[31], DocUNet [28], CU-Net [38]---就是简单的N个UNet

本文中使用嵌套的UNet而不是，级联的UNet

<img src="https://s2.loli.net/2022/11/05/6oaAOv27XmMRpwb.png" alt="image.png" style="zoom:80%;" />

本图结构：

1. 六个encoder
2. 五个decoder
3. 最右侧的特征融合模块，附着这编码和解码模块

<font color="blue" size="5">具体细节</font>

- enocder stages

**EN_1-4**中：我们分别使用之前提到的RSU-7 , RSU-6, RSU-5, RSU-4。L通常是根据特征图的分辨率进行设置的，分辨率越大，L越大，**EN_5-6**中特征图的分辨率通常很小，如果在这种小的分辨率中使用下采样的话就会造成有用内容的损失。因此这里使用RSU-4F，**在F系列中，我们使用膨胀卷积(也就是空洞卷积)代替原本的下采样层，这就意味着，RSU-4F中所有的特征层的分辨率是不变的**

同理，decoder层有对称的结构。

最后是saliency map fusion module：用了类似HED(Holistically-nested Edge Detection)的方法，我们有6个side output通过$S^{(n)}_{side}$来表示，他们通过一个3*3的卷积层和一个sigmod函数生成的，之后进行堆叠，最后通过1-1的卷积以及sigmod函数得到最终的限制特征层

## 5. deep supervision和损失函数

见HED

![111](https://img-blog.csdnimg.cn/20210629095442443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N0dWR5ZWJveQ==,size_16,color_FFFFFF,t_70)



![111](https://img-blog.csdnimg.cn/20210629095516787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N0dWR5ZWJveQ==,size_16,color_FFFFFF,t_70)

https://blog.csdn.net/studyeboy/article/details/118326010

下图为损失函数![image.png](https://s2.loli.net/2022/11/06/ehmQWCbLHs8KDc3.png)

- $l_{side}$为sup对应m的损失函数
- $l_{fuse}$是$sup_{fuse}$的损失函数
- w是对应的weight

**具体的loss**

- 也就是cossentropy

![image.png](https://s2.loli.net/2022/11/06/HDJ5FrREkvZ8lbU.png)

- (r , c)是坐标哦
- (H , W)是image size
- $P_{G(r,c)}$and$P_{S(r,c)}$分别代表pixel values of the ground truth and the predicted saliency probability map

- In the testing process, we choose the fusion output $l_{fuse}$ as our ﬁnal saliency map.
