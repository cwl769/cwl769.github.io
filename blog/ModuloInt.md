在一些需要对答案取模的题目中，我们需要每次都进行一次取模，非常的麻烦，所以我发明了下面这款 `Int` 。

```cpp
typedef long long int64;
typedef long long unsigned uint64;
class Int
{
private:
    constexpr static int64 M = 1000000007ll;
    Int(int64 x, bool y):val(x){}
public:
    int64 val;
    Int():val(){}
    Int(int x):val((x%M+M)%M){}
    Int(int64 x):val((x%M+M)%M){}
    Int(unsigned x):val(x%M){}
    Int(uint64 x):val(x%M){}
    Int operator + (const Int& b){return val + b.val;}
    Int operator - (const Int& b){return val - b.val;}
    Int operator * (const Int& b){return val * b.val;}
    Int operator - (){return (val==0)?Int():Int(M-val, 1);}
    Int operator ^ (int64 b)
    {
        int64 base = val, ans = 1ll;
        for(;b;b>>=1)
        {
            if(b&1)ans = ans * base % M;
            base = base * base % M;
        }
        return Int(ans, 1);
    }
    void operator += (const Int& b){val = (val + b.val) % M;}
    void operator -= (const Int& b){val = ((val-b.val)%M+M)%M;}
    void operator *= (const Int& b){val = (val * b.val) % M;}
};
```

1.   `998244353` 可以替换成需要的模数。

2.   私有构造函数 `Int(int64, bool)` 是在保证 $x\in [0,M)$ 的情况下减少取模运算次数。

3.   在 C++ 中 `^` 运算符**优先级**低于 `*` ，与数学上习惯相反，使用时要注意。