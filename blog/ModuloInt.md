在一些需要对答案取模的题目中，我们需要每次都进行一次取模，非常的麻烦(~~yjd同学曾经深受其害~~)，所以我发明了下面这款 `Int` 。

```
typedef long long int64;
struct Int
{
    static int64 MOD;
    int64 val;
    Int(){val=0ll;}
    Int(int x){val=(x%MOD+MOD)%MOD;}
    Int(int64 x){val=(x%MOD+MOD)%MOD;}
    Int operator + (Int b){return Int((val+b.val)%MOD);}
    Int operator - (Int b){return Int(((val-b.val)%MOD+MOD)%MOD);}
    Int operator * (Int b){return Int((val*b.val)%MOD);}
    void operator += (Int b){val=(val+b.val)%MOD;}
    void operator -= (Int b){val=((val-b.val)%MOD+MOD)%MOD;}
    void operator *= (Int b){val=(val*b.val)%MOD;}
};
int64 Int::MOD = 998244353;
```

**其中 `998244353` 可以替换成需要的模数**。