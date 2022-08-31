# transformation

### Scale

缩放变换

![image.png](https://s2.loli.net/2022/08/28/GNUmYqMLEioyz2b.png)



这就是一个缩放矩阵

如果x,y方向是不规则缩放

![image.png](https://s2.loli.net/2022/08/28/xZeCflWNS9TmauH.png)

### Reflection Matric

![image.png](https://s2.loli.net/2022/08/28/2doIyMXagzj8ArE.png)

### Shear Matrix切变

![image.png](https://s2.loli.net/2022/08/28/Uiy7ZTJqtKI8gAN.png)

### Rotate 旋转

一般都是原点旋转，一般都是逆时针旋转

![image.png](https://s2.loli.net/2022/08/28/yA7uzaVNxkt5OD4.png)

![image.png](https://s2.loli.net/2022/08/28/BTmYCojWzSAvIRL.png)

## Linear transforms = Matrices(if the same dimension)

**线性变换**

![image.png](https://s2.loli.net/2022/08/28/IzNiAl1OCYsj8oD.png)

## 齐次坐标

在平移变换中

![image.png](https://s2.loli.net/2022/08/28/jg3YfFTXJbRN7yz.png)

![image.png](https://s2.loli.net/2022/08/28/78TfpFEDdbzL3cx.png)

## 解决方法

为了解决平移变换中，方式不一样的问题，我们添加了一个维度

![image.png](https://s2.loli.net/2022/08/28/vYKps7Ctf8GBmVX.png)

但是我们需要区分点的平移和向量的平移：向量的平移不改变向量的！！！！

- vector + vector = vector
- point - point = vector
- point + vector = point
- point + point = ??

![image.png](https://s2.loli.net/2022/08/28/SgPd1lan5x4CYiz.png)

point + point 就是这两个点的中点！！！

$x` = {x_1+x_2 \over 2}  $

## 反射变换

![image.png](https://s2.loli.net/2022/08/28/Cnolr2miucbSGp3.png)

![image.png](https://s2.loli.net/2022/08/28/WmGPNgZ2AerjhTf.png)

## 逆变换

![image.png](https://s2.loli.net/2022/08/28/VFejZxQT2w1v9td.png)

## 组合变换

变换的顺序是不是改变的

![image.png](https://s2.loli.net/2022/08/28/GyM3E2dDWB9S8x4.png)

![image.png](https://s2.loli.net/2022/08/28/u3vwHoplf5n7PSV.png)

![image-20220828164105482](img/image-20220828164105482.png)

## 分解变换

**如果不是在原点旋转**

![image.png](https://s2.loli.net/2022/08/28/cjoM9SmuG6nBWNY.png)

## 3D变换

3的变换也可以多加上一维

![image.png](https://s2.loli.net/2022/08/28/1i7LhqDsMxYdFmE.png)

![image.png](https://s2.loli.net/2022/08/28/mtrQI7esOpxHwfC.png)

答案：先进行线性变换，再进行平移同2D



>![image.png](https://s2.loli.net/2022/08/28/Cnolr2miucbSGp3.png)

## 补充

![image.png](https://s2.loli.net/2022/08/28/AGecMUDmXbZ5KSi.png)