在一些需要对答案取模的题目中，我们需要每次都进行一次取模，非常的麻烦，所以我发明了下面这款 `Int` 。

```cpp
typedef long long int64;
typedef long long unsigned uint64;
template<int64 M>
class Int {
private:
    int64 v;
public:
    Int():v(){}
    Int(const Int& b) {
        v = b.v;
    }
    Int(Int&& b) {
        v = b.v;
    }
    Int(int64 x) {
        if(x<0) {
            v = (x%M)+M;
            if(v==M)v = 0;
        } else if (x >= M) {
            v = x % M;
        } else {
            v = x;
        }
    }
    Int& operator = (const Int& b) {
        v = b.v;
        return *this;
    }
    Int& operator = (Int&& b) {
        v = b.v;
        return *this;
    }
    Int operator + (Int b) const {
        return v + b.v;
    }
    Int operator - (Int b) const {
        return v - b.v;
    }
    Int operator * (Int b) const {
        return v * b.v;
    }
    Int& operator += (Int b) {
        v += b.v;
        if(v >= M)
            v -= M;
        return *this;
    }
    Int& operator -= (Int b) {
        v -= b.v;
        if(v < 0)
            v += M;
        return *this;
    }
    Int& operator *= (Int b) {
        v = (v * b.v) % M;
        return *this;
    }
    Int operator ^ (uint64 b) const {
        Int ans = 1;
        Int base = *this;
        for(;b;b>>=1) {
            if(b&1)
                ans *= base;
            base *= base;
        }
        return ans;
    }
    int64 val() const {return v;}
};
```

**tips:**

1.   在 C++ 中 `^` 运算符**优先级**低于 `*` ，与数学上习惯相反，使用时要注意。