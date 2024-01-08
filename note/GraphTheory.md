### 概念

#### 子图

对于原图 $G = (V, E)$，

定义：

子图 (Subgraph)：$G'=(V',E'),V'\subset V,E'\subset E$。

生成子图 (Spanning Subgraph)：$G'=(V,E'),E'\subset E$。

导出子图 (Induced Subgraph)：$G'=(V',E'),V'\subset V,E'\subset E,(u,v\in V'\rightarrow (u, v)\in E')$ 。
	即如果点 $u,v$ 在导出子图中，那么所有边 $(u, v)$ 都在导出子图中。
	上述导出子图可记作 $G[V']$