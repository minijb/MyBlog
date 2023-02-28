# quick-find算法

问题：

- 输入是一列整数对，每个整数都代表着某种类型的对象
- 一对整数`p q`代表着p和q相连

- 像这种p q的点就是一个触电

- 我们如何直到在一个大型网络中，两个触电是否相连

![image.png](https://s2.loli.net/2022/09/15/OngVeAaq9KmRsPT.png)

这种算法可以应用到很多方面

- 网络
- 像素点
- 人际关系

**connection的抽象概念**

- 自反性 reflexive
- 对称性
- 传递性

**connected component**:连通分量：maximal set of objects that are mutually connected

![image.png](https://s2.loli.net/2022/09/15/dLUEQpye1CRmJni.png)

为了解决这个问题我们将它封装到一个API中

Union-find算法API

```
UF(int N) 			initialize union-find data structure with N object
void union(int p , int q) 			add connection between p and q
boolean connected(int p , int q) 			are p and q in the same component?
int find(int p) 			componet identifier for p 
int count() 			number of components
```

## 1. quick-find 算法

Data structure

- Integer array id[] of size N
- Interpretation: p and q are connected iff they have the save id

```
		0	1	2	3	4	5	6	7	8	9
id[] 	0	1 	1	8	8	0	0	1	8	8
```

![image.png](https://s2.loli.net/2022/09/15/fMKJzw5D91i6U2j.png)

**find**: check if p and q have the same id

**union**: to merge components contatining p and q , change all entries whose id equals id[p] and id[q]

union的过程

```
union(4,3)
		3	4
id[]	3	4

		3	4
id[]	3	3
```

**具体算法**

```python
public class QuickFindUF {
    private int[] id;

    public QuickFindUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++) {
            id[i] = i;
        }
    }

    public boolean connected(int p, int q) {
        return id[p] == id[q];
    }

    public void union(int p, int q) {
        int pid = id[p];
        int qid = id[q];
        for (int i = 0; i < id.length; i++) {
            if(id[i]==pid) id[i] = qid;
        }
    }
}

```

### 算法分析

| algorithm | initialize | union | find |
| --------- | ---------- | ----- | ---- |
| quickfind | N          | N     | 1    |

Quick-find defect: union is too expensive

如果我们最后构建出只包含少量的components的模式，那么就要调用N-1次union此时时间复杂度为$N^2$

