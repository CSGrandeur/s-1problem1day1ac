## 筛质数

### 埃氏筛法

埃拉托斯特尼筛法（sieve of Eratosthenes）

`O(nloglogn)`

```cpp
bool isPrime[maxn];
void SetPrime()
{
    memset(isPrime, 1, sizeof(isPrime));
    isPrime[0] = isPrime[1] = false;
    for(int i = 2; i * i < maxn; i ++)
    {
        if(!isPrime[i])
            continue;
        for(int j = i * i; j < maxn; j += i)
            isPrime[j] = false;
    }
}
```

### 欧拉线性筛法

`O(n)`

```cpp
bool isPrime[maxn];
int prime[maxm], ptp
void SetPrime()
{
    ptp = 0;
    memset(isPrime, 1, sizeof(isPrime));
    isPrime[0] = isPrime[1] = false;
    for(int i = 2; i < maxn; i ++)
    {
        if(isPrime[i]) prime[ptp ++] = i;
        for(int j = 0; j < ptp && i * prime[j] < maxn; j ++)
        {
            isPrime[i * prime[j]] = false;
            if(i % prime[j] == 0) break;
        }
    }
}
```
