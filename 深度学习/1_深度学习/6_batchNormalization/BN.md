error surface比较崎岖的时候我们是比较难以训练的，那么如果我们能够不让它那么崎岖，是不是就可以更好的训练

如下图所示，不同的特征如果斜率相差较大，那么就不利于寻训练，我们可以找一个方法让他们斜率差别不那么大

<img src="https://s2.loli.net/2022/10/24/hJaIg2mDtvsAueO.png" alt="image.png" style="zoom:33%;" />

我们可以让输入在一定的范围内，这就是Feature Normalization

## 1. normalization

正则化过程：

<img src="https://s2.loli.net/2022/10/24/8Mt5zwNvPOTuQ1e.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/24/qGHv5raISY64bBf.png" alt="image.png" style="zoom:33%;" />

此时我们需要将BN层作为network的一部分，因为随着参数的更新，其平均值和方差也会跟着更新

<img src="https://s2.loli.net/2022/10/24/xl7mBWiQcPfsMwY.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/24/m8zkNpUKOTHjsXw.png" alt="image.png" style="zoom:33%;" />

##  2. BN-testing

在test中我们通常没有一个batch的数值用来计算平均值和方差，此时pytorch会计算一个moving average

<img src="https://s2.loli.net/2022/10/24/OJ7SM2uRkKwn4Zi.png" alt="image.png" style="zoom:33%;" />

## 3. 常见的normalization

<img src="https://s2.loli.net/2022/10/24/5U4GWH96XDxndbZ.png" alt="image.png" style="zoom:33%;" />

