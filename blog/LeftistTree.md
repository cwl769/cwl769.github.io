## 左偏树（可并堆）

### 约定

- 本文中 $\log$ 一般以 $2$ 为底。
- 示例代码及讲解均为小根堆

### dist 的定义与性质[^1]

#### 定义

- 左儿子或右儿子为空的二叉树节点为**外节点**。

- 外节点的 `dist` 值为 $1$。

- 空节点的 `dist` 值为 $0$。

- 其余节点的 `dist` 值为 $\min (dist_{左儿子}, dist_{右儿子}) + 1$。

由上述定义可知 `dist` 可理解为到最近空节点的距离。

#### 性质

- 任意一颗具有 $n$ 个节点的二叉树，其根节点的 `dist` 值 $\leq \lceil \log (n+1) \rceil$。

**证明:**

考虑最坏情况，即一颗满二叉树。除此之外的任何情况都可能导致根节点距离一个空节点更近。

由满二叉树性质可知 根节点的 `dist` 值为 $\lceil \log (n+1) \rceil$。

故 $dist_{根节点}\leq\lceil \log (n+1) \rceil$。

**注意以上所述内容对任意二叉树均成立**。



### 左偏树[^2]

左偏树是一颗二叉树，满足对于任意节点都有其左儿子的 `dist` 值 $\geq$ 右儿子的 `dist` 值，即 $dist_{lson} \geq dist_{rson}$ ，故称其为“左偏”。

左偏树是一种可并堆，支持堆的基本操作（插入 获取堆顶 弹出堆顶），还支持合并操作。

#### 合并

设有两颗左偏树 $A$ 、$B$，根分别为 $root_{A}, root_{B}$，其合并流程为（以小根堆为例）：

1. 选择 $root_{A}, root_{B}$ 中较小的作为合并后的根节点 $root$ （这里不妨设 $root_{A} \leq root_{B}$ ）。
2. 递归的合并 根节点的右儿子和 $B$ ，即 $merge(lson_{root}, B)$。
3. 更新根节点的 `dist` 值，维护左偏性质。

##### 复杂度

$O(\log N)$。

上述合并过程本质上是从根节点沿右儿子走向一个空节点，由 `dist` 性质可得递归层数最多为 $\lceil \log (n+1) \rceil$ ，于是复杂度得证。

#### 随机合并[^3]

观察合并过程，发现维护左偏的目的是走向 `dist` 更小的一个儿子，由此可以放弃存储 `dist` 改用另外的方式实现上述过程，即采用随机走向左右儿子方式。

```cpp
Node* merge(Node* a, Node* b)
{
    if(a==nullptr)return b;
    if(b==nullptr)return a;
    if(a->val > b->val)swap(a, b);
    if(myrand()&1)swap(a->son[0], a->son[1]);//在这里随机选择走向哪个儿子
    a->son[1] = merge(a->son[1], b);
    return a;
}
```

##### 复杂度

期望复杂度 $O(\log N)$。

**证明:**

设 $N_{T}$ 表示以 $T$ 为根的二叉树的节点个数。

设根为 $T$ 的二叉树左儿子为 $Ls$ 右儿子为 $Rs$。

设节点 $T$ 的父节点为 $fa_{T}$。

设根为 $T$ 的二叉树期望递归深度为 $Eh(T)$。

若 $Ls$ 与 $Rs$ 满足
$$
\begin{gather}
Eh(Ls) \leq \log(N_{L} + 1) \nonumber\\
Eh(Rs) \leq \log(N_{R} + 1) \nonumber
\end{gather}
$$
则
$$
\begin{equation}
\begin{aligned}
Eh(T) &= 1 + \frac{Eh(Ls) + Eh(Rs)}{2} \\
&\leq 1 + \frac{\log ((N_{L}+1)(N_R+1))}{2} \\
&= 1 + \log \sqrt{(N_{L}+1)(N_R+1)} \\
&= \log\left( 2 * \sqrt{(N_{L}+1)(N_R+1)} \right) \\
&\leq \log(N_{L} + N_{R} + 2) \\
&= \log(N_{T} + 1)
\end{aligned}\nonumber
\end{equation}
$$
即
$$
Eh(Ls) \leq \log(N_{L} + 1), Eh(Rs) \leq \log(N_{R} + 1) \implies Eh(T) \leq \log(N_{T} + 1)
$$
由于叶节点满足
$$
Eh(LEAVE) = 1 \leq \log(1 + 1)
$$
由归纳法可证得根节点 $ROOT$ 的期望递归深度为
$$
Eh(ROOT) \leq \log(N_{ROOT} + 1)
$$


#### 取堆顶

返回根节点值即可

```cpp
inline int64 top() const {return root->val;}
```

#### 弹出堆顶

合并根节点的左右子树，然后删除旧根节点

```cpp
void pop()
{
    Node *old_root = root;
    root = merge(root->son[0], root->son[1]);
    delete old_root;old_root = nullptr;
}
```

#### 插入

新建一颗左偏树，其根节点值为要插入的值，然后合并两棵树

```cpp
void insert(int64 v)
{
    Node *node = new Node(v);
    root = merge(root, node);
}
```

&nbsp;

&nbsp;

&nbsp;

---

##### 完整代码

```cpp
class LeftistTree
{
private:
    class Node
    {
    private:
        int64 val;
        Node *son[2];
    public:
        Node():val(),son{nullptr, nullptr}{}
        Node(int64 v):val(v),son{nullptr, nullptr}{}
        ~Node(){}
        friend class LeftistTree;
        friend Node* merge(Node* a, Node* b)
        {
            if(a==nullptr)return b;
            if(b==nullptr)return a;
            if(a->val > b->val)swap(a, b);
            if(myrand()&1)swap(a->son[0], a->son[1]);
            a->son[1] = merge(a->son[1], b);
            return a;
        }
    };

    Node * root;
public:
    LeftistTree():root(nullptr){}
    LeftistTree(int64 v){root = new Node(v);}
    ~LeftistTree(){}
    inline int64 top() const {return root->val;}
    void pop()
    {
        Node *old_root = root;
        root = merge(root->son[0], root->son[1]);
        delete old_root;old_root = nullptr;
    }
    void insert(int64 v)
    {
        Node *node = new Node(v);
        root = merge(root, node);
    }
    void operator += (LeftistTree& b)
    {
        root = merge(root, b.root);
    }
};
```

&nbsp;

&nbsp;

&nbsp;

[^1]:[OI-wiki dist的定义和性质](https://oi-wiki.org/ds/leftist-tree/#dist-%E7%9A%84%E5%AE%9A%E4%B9%89%E5%92%8C%E6%80%A7%E8%B4%A8)
[^2]: [OI-wiki 左偏树的定义和性质](https://oi-wiki.org/ds/leftist-tree/#%E5%B7%A6%E5%81%8F%E6%A0%91%E7%9A%84%E5%AE%9A%E4%B9%89%E5%92%8C%E6%80%A7%E8%B4%A8)
[^3]:[Randomized Heap](https://cp-algorithms.com/data_structures/randomized_heap.html)

