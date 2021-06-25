## 最长下降子序列（LDS）

`O(nlogn)`

```cpp
#include<functional>
#include<algorithm>
const int maxn = 1100000;
int dp[maxn], tp;
int LDS(int a[], int n)
{
    tp = 0;
    for(int i = 0; i < n; i ++)
    {
        int dpIth = 0;
        if(tp == -1 || dp[tp] > a[i])
            dpIth = ++ tp;
        else
            // 求上升子序列时去掉 `std::greater<int>()`
            dpIth = std::upper_bound(dp, dp + tp + 1, a[i], std::greater<int>()) - dp;
        dp[dpIth] = a[i];
    }
    return tp + 1;
}