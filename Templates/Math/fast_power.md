
## 快速幂取模

```cpp
// const int mod = ...
int PowMod(int a, int n, int mod=mod)
{
    int ret = 1;
    for(; n; n >>= 1, a = 1LL * a * a % mod)
        if(n & 1) ret = 1LL * ret * a % mod;
    return ret;
}
```
