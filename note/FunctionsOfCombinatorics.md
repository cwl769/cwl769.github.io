#### 树的拓扑序计数

$$
\frac{n!}{\prod size_{subtree(x)}}
$$

其中，$n$ 为树的节点个数，$x$ 为树的每一个节点

##### 证明: (待补充)

树的拓扑序只要保证每一个子树的根节点在最前面即可
**注意**选择不同根节点得到的拓扑序个数也可能不同

设节点 $x$, 其子树为 $y_i$ , 以 $x$ 为根的子树大小为 $size_x$
设 $f_x$ 表示以 $x$ 为根的子树的拓扑排序个数

有
$$
f_x = 
$$


##### 例题: 

[YBTOJ NOIP2023 Day2 C](https://www.ybtoj.com.cn/contest/477/problem/3)

![Screenshot](../source/screenshot/YBTOJ/NOIP2023_Day2_C/problem.png)