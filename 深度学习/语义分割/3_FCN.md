## 1. FCN

- 端对端
- 全卷积

## 2. 核心思想

在原本中，我们最后会将特征图拉开，然后进行全连接进行分类，本文中思考是否可以使用卷积来替代全连接层

<img src="https://s2.loli.net/2022/10/29/ps7leRLaFHUo36O.png" alt="image.png" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/10/29/LaW8qAY7dGpI3VC.png" alt="image.png" style="zoom:50%;" />

## 3. s代表什么

s代表上采样倍数

<img src="https://s2.loli.net/2022/10/29/hRPXBUm8YJ2cbaC.png" alt="image.png" style="zoom:50%;" />

