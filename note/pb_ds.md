#### 前置事项

必须包含头文件

```cpp
#include <ext/pb_ds/assoc_container.hpp>
```

命名空间

```cpp
__gnu_pbds::
```



#### Hash

头文件

```cpp
#include <ext/pb_ds/hash_policy.hpp>
```

声明

```cpp
__gnu_pbds::cc_hash_table<Key, Value> ;
__gnu_pbds::gp_hash_table<Key, Value> ;
```

`cc_hash_table`为拉链法

`gp_hash_table`为查探法

一般用`gp_hash_table`，会更快

**STL的unordered_map很慢，建议用`gp_hash_table`代替**



#### 平衡树

头文件

```cpp
#include <ext/pb_ds/tree_policy.hpp>
```

模板参数

```cpp
template <
    typename Key,
    typename Mapped,
    typename Cmp_Fn = std::less<Key>,
    typename Tag = rb_tree_tag,
    template <
        typename Node_CItr,
        typename Node_Itr,
        typename Cmp_Fn_,
        typename _Alloc_ >
    class Node_Update = null_node_update,
    typename Alloc = std::allocator<char> >
class tree;
```



