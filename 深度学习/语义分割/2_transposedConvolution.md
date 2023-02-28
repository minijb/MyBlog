## 1. Transposed Convolution的作用

作用：upsampling

- 转置卷积还是卷积

<img src="https://s2.loli.net/2022/10/29/A8cpnEuYFWKZVRw.png" alt="image.png" style="zoom:50%;" />

## 2. 转置卷积的步骤

- 在输入特征图元素间填充s-1行列的0
- 在输入特征图四周填充k-p-1行列的0
- 将卷积核参数上下，左右翻转
- 进行卷积操作(填充0，步距为1)

<img src="https://s2.loli.net/2022/10/29/VFYyw4vf3c2S5tU.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/29/FDKONEtCe63AYL7.png" alt="image.png" style="zoom:50%;" />