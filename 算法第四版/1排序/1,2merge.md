# merge

## 自顶向下

### 使用交换参数，避免辅助数组和主数组之间的复制

使用了辅助数组，通过不断交换参数来避免辅助数组的复制!!!

```c++
public static enum version{
    one,two;
}

public static void sort(Comparable[] a,version v) {
    switch(v) {
        case one:
            aux = new Comparable[a.length];
            sort(a, 0 , a.length-1);
            break;
        case two:
            sort(a.clone(),a,0,a.length-1);//通过交换参数避免对于辅助数组的复制
            break;

    }
}    

public static void sort(Comparable[] src , Comparable[] dst , int l , int r) {
    if (l >= r) return;
    int mid = (l + r) / 2;
    //不断转换两者的地位 -- 以此就不需要复制操作了
    sort(dst, src, l, mid);
    sort(dst, src, mid + 1, r);
    //根据递归的性质，但是在本层中 --- 顺序使不会该改变的 --- 无论交换多少次，最终的结果不会有差别
    merge(src, dst, l, mid, r);
}

public static void merge(Comparable[]src , Comparable[] dst, int l, int mid, int r) {
    int i = l, j = mid + 1;
    // System.arraycopy(a, l, aux, l, r - l + 1); 避免数组复制
    for (int k = l; k <= r; k++) {
      if (i > mid) dst[k] = src[j++];
      else if (j > r) dst[k] = src[i++];
      else if (less(src[i], src[j])) dst[k] = src[i++];
      else dst[k] = src[j++];
    }
}
```

### 在小数组中使用insert避免不断的叠加栈

```java
    public static void sort(Comparable[] a , int low , int high) {
        int mid = ( low + high ) /2;
        //小数组使用insert
        if((high - low) < 64){
            Insert.sort(a,low,high);
        }
        else{
            sort(a, low, mid);
            sort(a, mid+1, high);
            if( less(a[mid], a[mid+1]) ) return; 
            merge(a, low, mid, high); 
        }
    }
```

### 经典剪枝操作

因为归并中，左右数组排序之后是有序的，因此可以通过中间的两个值来判断当前数组是否有序

```java
    public static void sort(Comparable[] a , int low , int high) {
        int mid = ( low + high ) /2;
        if((high - low) < 64){
            Insert.sort(a,low,high);
        }
        else{
            sort(a, low, mid);
            sort(a, mid+1, high);
            if( less(a[mid], a[mid+1]) ) return; //因为部分有序，因此可以进行截断
            merge(a, low, mid, high); 
        }
    }
```

## 自底向上

自底向上不需要简直操作，但是可以使用insert排序小数据

### **基础的自底向上**

```java
public static void sortBottomToUp(Comparable[] a) {
    int N = a.length;
    aux = new Comparable[N];

    // sz -- 每次排序的数组长度
    for (int sz = 1; sz < N; sz = sz * 2) {
        for (int lo = 0; lo < N - sz; lo += sz * 2) {
            merge(a, lo, lo + sz - 1, Math.min(lo + sz + sz - 1, N - 1));
            // 确保不会超过数组
        }

    }
}
```

### **在小数组使用insert**

```java
public static void sortBottomToUp_WhitInsert(Comparable[] a) {
    // 直接使用insert对16以上的数组进行排序
    // 如果本身数组小于16，直接使用insert
    int N = a.length;

    if (N < 16) {
        Insert.sort(a);
        return;
    }

    aux = new Comparable[N];
    for (int i = 0; i < N; i += 16) {
        Insert.sort(a, i, Math.min(i + 15, N - 1));
        // StdOut.printf("%d --- %d\n",i,Math.min(i+15,N-1));
    }

    for (int sz = 16; sz < N; sz = sz * 2) {

        for (int lo = 0; lo < N - sz; lo += sz * 2) {
            merge(a, lo, lo + sz - 1, Math.min(lo + sz + sz - 1, N - 1));
            StdOut.printf("lo = %d , mid = %d , h = %d \n",lo, lo + sz - 1, Math.min(lo + sz + sz - 1, N - 1));
        }
    }

}
```

### **通过交换参数杜绝辅助数组**

```java
public static void sort_all(Comparable[] src) {
    // 每次交换两个数组的地位，因为没有递归，因此需要记录
    int flag = 0;// 记录主数组 0 --- src 1 --- dst
    int N = src.length;

    if (N < 16) {
        Insert.sort(src);
        return;
    }
    Comparable[] dst = src;

    aux = new Comparable[N];
    for (int i = 0; i < N; i += 16) {
        Insert.sort(src, i, Math.min(i + 15, N - 1));
    }

    for (int sz = 16; sz < N; sz = sz * 2) {
        for (int lo = 0; lo < N - sz; lo += sz * 2) {
            if (flag == 0) {
                merge(src, dst, lo, lo + sz - 1, Math.min(lo + sz * 2 - 1, N - 1));
            } else {
                merge(dst, src, lo, lo + sz - 1, Math.min(lo + sz * 2 - 1, N - 1));
            }

        }
        if (flag == 0 ) flag =1;
        else flag = 0;
    }

    if (flag == 1)
        src = dst;
}
```

### 次线性额外空间

- 先将M的数组分为N/M块
- 额外空间为`max(M,N/M)`
- 将块看作元素，第一个元素作为主键，块内使用选择排序进行排序
- 遍历数组，使用归并排序。

和自底向上思想差不多。

### 归并有序队列

自底向上的归并有序队列

### 自然的归并排序

当需要将两个子数组排序时能够利用数组中已经有序的部分。首先找到一个有序的子数组(移动指针指到当前元素比上一个元素小为止)，然受找到另一个有序数组进行
