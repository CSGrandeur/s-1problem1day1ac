
## 乘法逆元

### 方法1：费马小定理

`GCD(a, b)==1`，则`a^(b-1) % b == 1`，故 `a^(b-2)`为`a`模`b`的逆元，调用快速幂取模。

模数 `b` 为质数时可用。

```cpp
int FermatInv(int a, int b)
{
    return PowMod(a, b - 2, b);
}
```

### 方法2：扩展欧几里得

求逆元模数b通常很大，用long long保险，可行解存在引用参数x,y中，返回值为GCD(a,b)的结果。

最常用、安全的求逆元方式。

```cpp
typedef long long LL;  
LL ExGCD(LL a, LL b, LL &x, LL &y)
{
    if(!b)
    {
        x = 1, y = 0;
        return a;
    }
    LL d = ExGCD(b, a % b, x, y), t = x;
    x = y, y = t - a / b * x;
    return d; 
}
```

```cpp
int ExGcdInv(int a, int mod)
{
    LL x, y;
    ExGCD(a, mod, x, y);
    return (x % mod + mod) % mod;
}
```

### 方法3：递归求逆元

`mod` 必须为质数。

```cpp
LL Inv(LL a, LL mod)
{
    if(a == 1) return 1;
    return (mod - mod / a) * Inv(mod % a) % mod;
}
```

### 逆元打表

```cpp
int invList[maxn];
void GetInvList(int mod)
{
    invList[1] = 1;
    for(int i = 2; i < maxn; i ++)
        invList[i] = 1LL * (mod - mod / i) * invList[mod % i] % mod;
}
```
