## 板子

### 数据结构

#### 并查集

```cpp
struct DSU
{
    int *fa;
    int N;
    
    DSU(int n)
    {
        N = n + 10;
        fa = (int*)malloc(N*sizeof(int));
        for(int i=1;i<=n;++i)
        	fa[i] = i;
    }
    virtual ~DSU()
    {
        free(fa);fa=NULL;
    }
    int get(int x)
    {
        if(fa[x]==x)
        	return x;
       	return fa[x] = get(fa[x]);
    }
    inline void merge(int x, int y)
    {
        int rtx = get(x);
        int rty = get(y);
        if(rtx==rty)
        	return ;
        fa[rtx] = rty;
        return ;
    }
};
```

###### 复杂度

`DSU(int n);`  -> $O(N)$

`get(int x);`  -> $O(\log N)$

`merge(int x, int y);`  -> $O(\log N)$



#### 可持久化并查集

*基于可持久化数组*

```cpp
struct PersistentArray
{
    struct Node
    {
        int val;
        int lson, rson;
    };
    static Node pool[SIZE];
    static int pool_cnt;
    int newNode()
    {
        ++pool_cnt;pool[pool_cnt] = {0, 0, 0};
        return pool_cnt;
    }
    std::vector<int> root;
    int dL, dR;
    
    PersistentArray(int l, int r)
    {
        root.push_back(newNode());
        dL = l;dR = r;
        build(root[0], dL, dR);
    }
    void build(int rt, int l, int r)
    {
        if(l==r){pool[rt].val = l;return;}
        int mid = ((l+r)>>1);
        pool[rt].lson = newNode();build(pool[rt].lson, l, mid);
        pool[rt].rson = newNode();build(pool[rt].rson, mid+1, r);
    }
    int change(int rt, int p, int val)
    {
        int newrt = newNode();
        find_c(rt, newrt, dL, dR, p) = val;
        return newrt;
    }
    int& find_c(int rt, int newrt, int l, int r, const int p)
    {
        if(l==r){pool[newrt] = pool[rt];return pool[newrt].val;}
        int mid = ((l+r)>>1);
        pool[newrt] = pool[rt];
        if(p<=mid){pool[newrt].lson=newNode();return find_c(pool[rt].lson, pool[newrt].lson, l, mid, p);}
        else {pool[newrt].rson=newNode();return find_c(pool[rt].rson, pool[newrt].rson, mid+1, r, p);}
    }
    int find_q(int rt, int l, int r, int p)
    {
        if(l==r){return pool[rt].val;}
        int mid = ((l+r)>>1);
        if(p<=mid){return find_q(pool[rt].lson, l, mid, p);}
        else {return find_q(pool[rt].rson, mid+1, r, p);}
    }
    void print_s(int rt, int l, int r)
    {
        if(l==r){printf("%d ", pool[rt].val);return ;}
        int mid = ((l+r)>>1);
        print_s(pool[rt].lson, l, mid);
        print_s(pool[rt].rson, mid+1, r);
    }
    void print()
    {
        print_s(root[root.size()-1], dL, dR);
        printf("\n");
    }
};
PersistentArray::Node PersistentArray::pool[SIZE];
int PersistentArray::pool_cnt = -1;

int get(int x, PersistentArray& fa, int& root)
{
    int faa = fa.find_q(root, fa.dL, fa.dR, x);
    if(faa==x)
        return x;
    int anc = get(faa, fa, root);
    fa.pool[root] = fa.pool[fa.change(root, x, anc)]; /*tag1*/
    return anc;
}
```

在主函数中:

```cpp
int tmp = arr.root[id-1];
a = get(a, arr, tmp);
b = get(b, arr, tmp);
arr.root.push_back(tmp);
```

**WARNING: 上述片段的第74行(倒数第3行)，即末尾有tag1的一行 这一行很值得品位**

**此行在可持久化数组上修改了一个节点(即创建了一个新根)，并将旧根赋值为新根，这样保证一个版本的全部修改都在一个根上可以访问到**

![image](../source/picture/note/banzi001.gif)

**如上图为含有三个版本的并查集(由于路径压缩，查询也需要记入一个版本)**

**黑色线是各版本对应的根，而蓝色线为本版本实际应访问的入口**

**上述的赋值操作保证了从黑色线也能进入蓝线所指向的位置**

**实现上，黑线指向的位置在主函数中体现为 `tmp` 变量**



#### 树状数组

```cpp
template<typename T>
struct BIT
{
    T *c;
    int N;
    
    BIT(int n)
    {
        N = n + 10;
        c = (T*)calloc(N, sizeof(T));
    }
    ~BIT()
    {
        free(c);c=NULL;
    }
    void add(int x, T y)
    {
        for(;x<N;x+=(x&(-x)))c[x] += y;
    }
    T ask(int x)
    {
        T ans = 0;
        for(;x;x-=(x&(-x)))ans += c[x];
        return ans;
    }
};
```

**将add()和ask()内的for互换可改为求后缀**

###### 复杂度

`add(int x, T y);`  -> $O(\log N)$

`ask(int x);`  -> $O(\log N)$



#### 线段树

```cpp
struct SegmentTree
{
    struct Node
    {
        Node *lson, *rson;
        ll data;
        ll plus;

        Node()
        {
            lson = NULL;rson = NULL;
            data = plus = 0;
        }
        inline void update(const Node& lson, const Node& rson)
        {
            data = lson.data + rson.data;
            return;
        }
    };
    Node* root;
    int dl, dr;
    Node *pool;int pool_tot;

    SegmentTree(int l, int r)
    {
        dl = l;dr = r;
        root = new Node;
        createPool((r - l + 1 + 10)<<1);pool_tot = 0;
    }
    virtual ~SegmentTree()
    {
        delete[] pool;pool=NULL;
        delete root;root=NULL;
    }
    void createPool(int num)
    {
        pool = new Node[num];
    }
    Node* newNode()
    {
        return pool+(++pool_tot);
    }
    void build(Node& rt, int l, int r, const ll *arr)
    {
        if(l==r)
        {
            rt.data = arr[l];
            return;
        }
        int mid = ((l + r) >> 1);
        rt.lson = newNode();
        rt.rson = newNode();
        build(*rt.lson, l, mid, arr);
        build(*rt.rson, mid+1, r, arr);
        rt.update(*rt.lson, *rt.rson);
    }
    void spread(Node& rt, int l, int r)
    {
        (*rt.lson).plus += rt.plus;
        (*rt.rson).plus += rt.plus;
        int mid = ((l + r) >> 1);
        (*rt.lson).data += (ll)(mid - l + 1ll) * rt.plus;
        (*rt.rson).data += (ll)(r - mid) * rt.plus;
        rt.plus = 0;
    }
    void range_change(Node& rt, int l, int r, int ql, int qr, const ll& plus)
    {
        if(l==ql&&r==qr)
        {
            rt.plus += plus;
            rt.data += (ll)(r - l + 1ll) * plus;
            return;
        }
        spread(rt, l, r);
        int mid = ((l + r) >> 1);
        if(qr<=mid){range_change(*rt.lson, l, mid, ql, qr, plus);}
        else if(ql>mid){range_change(*rt.rson, mid+1, r, ql, qr, plus);}
        else 
        {
            range_change(*rt.lson, l, mid, ql, mid, plus);
            range_change(*rt.rson, mid+1, r, mid+1, qr, plus);
        }
        rt.update(*rt.lson, *rt.rson);
    }
    ll range_query(Node&rt, int l, int r, int ql, int qr)
    {
        if(l==ql&&r==qr)
        {
            return rt.data;
        }
        spread(rt, l, r);
        int mid = ((l + r) >> 1);
        if(qr<=mid){return range_query(*rt.lson, l, mid, ql, qr);}
        else if(ql>mid){return range_query(*rt.rson, mid+1, r, ql, qr);}
        else
        {
            return range_query(*rt.lson, l, mid, ql, mid) + range_query(*rt.rson, mid+1, r, mid+1, qr);
        }
    }
};
```

**以上为动态开点线段树的例子，以 洛谷P3372 为例 。实际应用中线段树灵活性强，需要结合题意设计**

**以上例子使用了内存池 实测用new开辟会慢一些**

###### 复杂度

`build(Node& rt, int l, int r, const ll *arr);`  -> $O(N)$

`spread(Node& rt, int l, int r);`  -> $O(1)$

`range_change(Node& rt, int l, int r, int ql, int qr, const ll& plus);`  -> $O(\log N)$

`range_query(Node&rt, int l, int r, int ql, int qr);`  -> $O(\log N)$



#### 可持久化数组（线段树）[^1]

```cpp
template<typename T>
struct PersistentArray
{
    struct Node
    {
        T val;
        Node *lson, *rson;
    };
    Node* newNode()
    {
        static Node Node_pool[10000000];
        static int poolcnt = -1;
        return Node_pool + (++poolcnt);
    }
    std::vector<Node*> root;
    int dL, dR;

    PersistentArray(int l, int r)
    {
        root.clear();root.push_back(newNode());
        dL = l;dR = r;
    }
    ~PersistentArray(){}
    void build(Node *rt, int l, int r)
    {
        if(l==r){return;}
        int mid = ((l+r)>>1);
        rt->lson = newNode();
        rt->rson = newNode();
        build(rt->lson, l, mid);
        build(rt->rson, mid+1, r);
    }
    void build(Node *rt, int l, int r, T* arr)
    {
        if(l==r){rt->val = arr[l];return;}
        int mid = ((l+r)>>1);
        rt->lson = newNode();
        rt->rson = newNode();
        build(rt->lson, l, mid, arr);
        build(rt->rson, mid+1, r, arr);
    }
    T& findp(Node* rt, Node* newrt, int l, int r, const int p)
    {
        if(l==r){(*newrt) = (*rt);return newrt->val;}
        int mid = ((l+r)>>1);
        (*newrt) = (*rt);
        if(p<=mid)
        {
            newrt->lson = newNode();
            return findp(rt->lson, newrt->lson, l, mid, p);
        }
        else
        {
            newrt->rson = newNode();
            return findp(rt->rson, newrt->rson, mid+1, r, p);
        }
    }
};
```

**提供了空初始化和数组初始化两种build方式**

**`findp()`函数返回引用，故既可当作右值，又可当作左值**

###### 复杂度

`build()`  -> $O(N)$  (相当于访问二叉树的每一个节点 )

`findp()`  -> $O(\log N)$ 



### 图论

#### 邻接表

```cpp
struct Graph
{
    struct Edge{int next, to, val;};
	std::vector<Edge> edg;
    int *h, N;
    
    Graph(int n)
    {
        N = n + 10;
        h = (int*)calloc(N, sizeof(int));
        edg.clear();
        edg.push_back({0,0,0});
        edg.push_back({0,0,0});//此处是为了使下标从2开始
    }
    ~Graph(){free(h);h=NULL;}
    void addedg(int x, int y, int z)
    {
        edg.push_back({.next=h[x], .to=y, .val=z});
        h[x] = edg.size() - 1;
    }
};
```

**构造函数中一定要插入至少一次（两次是为了无向边的奇偶变换），否则会使第一条边编号为 $0$ 从而无法访问**

###### 复杂度

`addedg(int x, int y, int z)`  -> $O(1)$



#### LCA(dfs序 + ST) 

参考:[luogu 上关于此的介绍](https://www.luogu.com.cn/blog/AlexWei/leng-men-ke-ji-dfs-xu-qiu-lca)

```cpp
std::vector<int> dep_bydfs;
std::vector<int> dfs_seq;
int dfn[500010], dfn_tot=0;
int faa[500010];
void dfs(int rt, int fa, const Graph& g, int dep)
{
    faa[rt] = fa;
    dfn[rt] = ++dfn_tot;
    dep_bydfs.push_back(dep);
    dfs_seq.push_back(rt);
    for(int i=g.h[rt],y;i;i=g.edg[i].next)
    {
        y = g.edg[i].to;
        if(y==fa)continue;
        dfs(y, rt, g, dep+1);
    }
}
int dep_st[500010][20];
int dep_st_posi[500010][20];
void lca_init(const Graph& g, const int& n)
{
    dep_bydfs.push_back(0);
    dfs_seq.push_back(0);
    dfs(ROOT, 0, g, 1);
    for(int i=1;i<=n;++i)
    {
        dep_st[i][0] = dep_bydfs[i];
        dep_st_posi[i][0] = dfs_seq[i];
    }
    for(int j=1;j<20;++j)
        for(int i=1;i<=n-(1<<j)+1;++i)
        {
            dep_st[i][j] = wlmin(dep_st[i][j-1], dep_st[i+(1<<(j-1))][j-1]);
            if(dep_st[i][j-1]<=dep_st[i+(1<<(j-1))][j-1])
            {
                dep_st[i][j] = dep_st[i][j-1];
                dep_st_posi[i][j] = dep_st_posi[i][j-1];
            }
            else
            {
                dep_st[i][j] = dep_st[i+(1<<(j-1))][j-1];
                dep_st_posi[i][j] = dep_st_posi[i+(1<<(j-1))][j-1];
            }
        }
    return ;
}
int lca(int x, int y)
{
    if(x==y)return x;
    int a = dfn[x], b=dfn[y];if(a>b)wlswap(a,b);++a;
    int len = log2(b - a + 1);
    int amin=dep_st[a][len], aminp=dep_st_posi[a][len];
    int bmin=dep_st[b - (1<<len) + 1][len], bminp=dep_st_posi[b - (1<<len) + 1][len];
    if(amin<=bmin){return faa[aminp];}
    else {return faa[bminp];}
}
```





### 杂项

#### 离散化

```cpp
template<typename T>
struct Discrete
{
    std::vector<T> data;

    inline void add(T x){data.push_back(x);}
    inline void discrete()
    {
        std::sort(data.begin(), data.end());
        data.resize(std::unique(data.begin(), data.end()) - data.begin());
    }
    inline int id(T x){return std::lower_bound(data.begin(), data.end(), x) - data.begin() + 1;}
    inline T val(int id){return data[id - 1];}
};
```

###### 复杂度

`add(T x);`  -> $O(1)$

`discrete()`  -> $O(N\log N)$

`id(T x)`  -> $O(\log N)$

`val(int id)`  -> $O(1)$

**此模板离散化后的值下标从 $1$ 开始**



#### 最值

```cpp
template<typename T> inline T wlmin(T a, T b){return (a<b)?a:b;}
template<typename T> inline T wlmax(T a, T b){return (a<b)?b:a;}
```

###### 复杂度

均为 $O(1)$



#### ST表

```cpp
inline int log2(int x){int cnt=-1;for(;x;x>>=1)++cnt;return cnt;}
template<typename T>
struct ST_table
{
    T* data;
    T** st;
    int N;
    T (*FUNC)(T a, T b);

    ST_table(T* ori, int n, T (*func)(T a, T b))
    {
        data = ori;
        N = n;FUNC = func;
        int t = log2(N);
        st = (T**)malloc((t+1)*sizeof(T*));
        for(int i=0;i<=t;++i){st[i] = (T*)calloc(n+1, sizeof(T));}
        init();
    }
    ~ST_table()
    {
        int t = log2(N);
        for(int i=0;i<=t;++i){free(st[i]);st[i]=NULL;}
        free(st);st=NULL;
    }
    void init()
    {
        for(int i=1;i<=N;++i)
        {
            st[0][i] = data[i];
        }
        int t = log2(N);
        for(int i=1;i<=t;++i)
        {
            int m = (1<<i);int m_half = (1<<(i-1));
            int lim = N - m + 1;
            for(int l=1;l<=lim;++l)
                {st[i][l] = FUNC(st[i-1][l], st[i-1][l+m_half]);}
        }
    }
    T query(int l, int r)
    {
        int len = r - l + 1;
        int t = log2(len);
        return FUNC(st[t][l], st[t][r - (1<<t) + 1]);
    }
};
```

###### 例题

[Balanced Lineup G](https://www.luogu.com.cn/problem/P2880)

###### 复杂度

`init()`  -> $O(N\log N)$

`query(int l, int r)`  -> $O(1)$



[^1]: 可能存在缺陷，需要更新
