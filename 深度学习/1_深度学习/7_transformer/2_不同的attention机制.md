## 1. local attention/ Truncated attention

如果我们的关注点只在一定的范围内，比如前后两边，我们可以缩减矩阵

<img src="https://s2.loli.net/2022/10/28/4prlVQR3M2gWdLv.png" alt="image.png" style="zoom:50%;" />

## 2. Stride attention

<img src="https://s2.loli.net/2022/10/28/FfWtAesp9IDMzQP.png" alt="image.png" style="zoom:50%;" />

## 3. global attention

我们可以添加一些特殊的token作为全局token

- attend to every token -> collect global information
- attended by evert token -> it knows global information

有两种方法，1. 选择原本就有的token作为global 2.外插一些作为global token

<img src="https://s2.loli.net/2022/10/28/1OBHoYFX7v8AErR.png" alt="image.png" style="zoom:50%;" />

> 我们可以使用多头技术，混合的使用之前的技术
>
> <img src="https://s2.loli.net/2022/10/28/XDitW8M94cjymrG.png" alt="image.png" style="zoom:33%;" />

## 4. focus on critical part---Clustering

如果某些位置的分数比较低，我们可以直接将它设置为0

<img src="https://s2.loli.net/2022/10/28/bcu76MSkhBxJ321.png" alt="image.png" style="zoom:50%;" />

我们只计算同一个位置的query和key，其他设为0

<img src="https://s2.loli.net/2022/10/28/PMmNZXU5ugBQzno.png" alt="image.png" style="zoom:50%;" />

## 5. 机器自己确定

<img src="https://s2.loli.net/2022/10/28/kAyoxSsaTr9bhuJ.png" alt="image.png" style="zoom:50%;" />

## 6. Linformer

<img src="img/image-20221028164508656.png" alt="image-20221028164508656" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/10/28/JLeKnq1ahNwFAib.png" alt="image.png" style="zoom:50%;" />



