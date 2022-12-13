## 1. 解决CNN的问题

<img src="https://s2.loli.net/2022/10/22/sjUQAZk6YPqHcid.png" alt="image.png" style="zoom:33%;" />

### 1.1 how to transform an image/feature map

<img src="https://s2.loli.net/2022/10/22/1akXKbfqFQBR7ld.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/22/DfsYxWlQXuBia6q.png" alt="image.png" style="zoom:33%;" />

**一些问题**

如果参数是一些小数，此时之前的例子就比较难实现，这样的话就不好做梯度下降

那么我们可以通过计算周围点到此点的距离来计算下一个点的值！！！！

<img src="https://s2.loli.net/2022/10/22/V15xlmTLKqzXwiP.png" alt="image.png" style="zoom:33%;" />

此时我们的模型就是这样

<img src="https://s2.loli.net/2022/10/22/VSIjzGxQ5fhJcH3.png" alt="image.png" style="zoom:33%;" />

<img src="https://s2.loli.net/2022/10/22/xfvHqYnsPBkoG5F.png" alt="image.png" style="zoom:33%;" />