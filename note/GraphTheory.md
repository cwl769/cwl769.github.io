### 概念

#### 子图

对于原图 $G = (V, E)$，

定义：

子图 (Subgraph)：$G'=(V',E'),V'\subset V,E'\subset E$。

生成子图 (Spanning Subgraph)：$G'=(V,E'),E'\subset E$。

导出子图 (Induced Subgraph)：$G'=(V',E'),V'\subset V,E'\subset E,(u,v\in V'\rightarrow (u, v)\in E')$ 。
	即如果点 $u,v$ 在导出子图中，那么所有边 $(u, v)$ 都在导出子图中。
	上述导出子图可记作 $G[V']$

极小连通子图：给定 $V' \subset V$，使得 $V'$ 连通的最小子图 $G'$ 称为极小连通子图，是包含 $V'$ 所有点的极小生成树

### tips

1.   无根树需要对一个节点所有邻节点操作。会被菊花图卡成 $O(n^2)$，可以定根后只更新父亲，可大大减小复杂度。
2.   求树上点集的极小连通子图，可以如下操作
     1.   将点集按照 dfs 序排成环
     2.   计算环上相邻点的距离和 $S_d$（可认为边权为 $1$ ）
     3.   $\frac{S_d}{2}$ 即为极小连通子图的边权和，加 $1$ 即可得到点数

