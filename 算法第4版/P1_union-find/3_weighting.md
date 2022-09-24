# Weighting quick-union

- modify quick-union to avoid tall trees
- keep track of size of each tree(number of object)
- Balance by linking root of smaller tree to root of larger tree

![image.png](https://s2.loli.net/2022/09/18/nslIN8aS9tMXUQ2.png)

也就是说，如果有一个大树和一个小树在一起，小树应该总是连到大树的下面，而不是反过来

![image.png](https://s2.loli.net/2022/09/18/ljUi9COcuIsAqZY.png)

### modify

data structure: same as quick-union , but maintain extra array sz[i] to count number of objects in the tree rooted by i

find : identical to quick-union

union:  

- link root of small tree to root of larger tree
- update sz[] array

![image.png](https://s2.loli.net/2022/09/18/Xle4ANUMWPiB3LO.png)

![image.png](https://s2.loli.net/2022/09/18/6LdKY3TmcrZfEVv.png)

```java
public class weighting {
    private int[] id;
    private int[] sz;

    public weighting(int N) {
        id = new int[N];
        sz = new int[N];
        for (int i = 0; i < N; i++) {
            id[i] = i;
            sz[i] = 1;
        }
    }

    private int root(int i) {
        while (i != id[i]) i = id[i];
        return i;
    }

    public boolean connected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int i = root(p);
        int j = root(q);
        if (i == j) return;
        if (sz[i] < sz[j]) {
            id[i] = j;
            sz[j] += sz[i];
        }else {
            id[j] = i;
            sz[i] += sz[j];
        }
    }

}

```

![image.png](https://s2.loli.net/2022/09/18/BqnV4t2PGeSiAls.png)

### path compression

![image.png](https://s2.loli.net/2022/09/18/a5TFg9yJrqU2NEG.png)

![image.png](https://s2.loli.net/2022/09/18/lQLAz75qKHpgYOW.png)

![image.png](https://s2.loli.net/2022/09/18/i5mXdLKueOHtZcC.png)