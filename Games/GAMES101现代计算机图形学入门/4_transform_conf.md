- 视图变换
- 投影变换
  - 正交变换
  - 透视变换

## 将2D变换映射到3D

![image.png](https://s2.loli.net/2022/08/28/FM8hSjoK9tOBgwI.png)

旋转！！

![image.png](https://s2.loli.net/2022/08/28/8IWvV37xpUQlPAi.png)

为什么y的负号是再左下角

根据坐标系：z叉乘x得到y（不是x叉乘z）因此y不一样

### 组合x,y,z的旋转

任意3D的旋转可以分解为x，y，z轴上的旋转

![image.png](https://s2.loli.net/2022/08/29/RgMrEfUSaVyp9xF.png)

### 罗德里德斯旋转公式

![image.png](https://s2.loli.net/2022/08/29/p5LfaF9hslnqbuA.png)

> 四元数主要是用来计算差值的

## Viewing变换

什么是视图变换？想想如何拍照

- 找到一个好的地方以及召集人（模型变换）
- 找一个角度来拍照（视图变换）
- 拍照(projection transform)

![image.png](https://s2.loli.net/2022/08/29/kFyJdLcUKi6xlC3.png)

up-direction用来固定自身的旋转！！！

![image.png](https://s2.loli.net/2022/08/29/V19qidvhaDk326G.png)

![image.png](https://s2.loli.net/2022/08/29/Tyxqwukjh2YSCM6.png)

![image.png](https://s2.loli.net/2022/08/29/mP6EybaD5zQ4YUZ.png)

> 关于线代的本质
>
> https://www.bilibili.com/video/BV1ib411t7YR?p=3&vd_source=8beb74be6b19124f110600d2ce0f3957

![image.png](https://s2.loli.net/2022/09/02/SyT4sFbBd83uZHn.png)

## 投影变换

- Orthographic projection正交投影
- Perspective projection 透视投影---现象：近大远小

![image.png](https://s2.loli.net/2022/09/02/bFAG6XfVJvyBDnP.png)

![image.png](https://s2.loli.net/2022/09/02/xmY6fwhGSavQc1J.png)

### 正交投影

![image.png](https://s2.loli.net/2022/09/02/eVoZ7hwXtz9kvg4.png)

![image.png](https://s2.loli.net/2022/09/02/XNz9deE86tgZWKc.png)

![image.png](https://s2.loli.net/2022/09/02/M2geEbc7ZfTya1Q.png)

![image.png](https://s2.loli.net/2022/09/02/2QLrbEVcFegyDSK.png)

![image.png](https://s2.loli.net/2022/09/02/sHMZ5bxuqYBhC2U.png)

### 透视投影

![image.png](https://s2.loli.net/2022/09/02/jkNBXLlQTvAS79I.png)

![](https://s2.loli.net/2022/09/02/FTehYbcvH5GE163.png)

![image.png](https://s2.loli.net/2022/09/02/u4KHn37J1QI6YTZ.png)

![image.png](https://s2.loli.net/2022/09/02/vXJDeotIubBlxwj.png)

![image.png](https://s2.loli.net/2022/09/02/St3hQNPTkX9p4Db.png)

![image.png](https://s2.loli.net/2022/09/02/gjy6b5vB4JP1umG.png)

![image.png](https://s2.loli.net/2022/09/02/gLifdaQBomJI2R9.png)

![image.png](https://s2.loli.net/2022/09/02/6dpgNn3Vser79IG.png)

![image.png](https://s2.loli.net/2022/09/02/pf1GPeTNvKqJlk2.png)

![image.png](https://s2.loli.net/2022/09/02/fCtTWkFeZnXE72A.png)