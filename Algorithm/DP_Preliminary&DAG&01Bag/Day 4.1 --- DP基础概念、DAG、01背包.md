# Day 4.1 --- DP基础概念、DAG、01背包
[动态规划.pptx](https://github.com/1282279732/s-1problem1day1ac/files/7136544/default.pptx)


## 一、DP基础概念

DP（Dynamic Programming）也叫动态规划。基本思想和分治法相似，将母问题分解多个子问题，通过子问题最优解，进而推出母问题最优解。



### 贪心法、分治法与DP的区别

贪心通过局部最优，进而解决母问题，但不一定最优。如果子问题相互独立，那么可采用分治法；若子问题互相影响，则采用DP。硬币找零的例子，图示解释分支法和DP面对子问题是怎么处理的。为什么子问题相互影响的时候我们采用DP？



### 小概念

1. 最优子结构

   最优子结构指的是，问题的最优解包含子问题的最优解。反过来说就是，我们可以通过子问题的最优解，推导出问题的最优解。

2. 无后效性

   无后效性，有两层含义，第一层含义是，在推导后面阶段状态的时候，我们只关心前面阶段的状态值，不关心这个状态是怎么一步步推导出来的。第二层含义是，某阶段状态一旦确定，就不受之后阶段的决策影响。

3. 重复子问题

   这个概念，用一句话概括就是： 不同的决策序列，到达某个相同的阶段时，可能会产生重复的状态。



### DP详细分析

动态规划四步骤

1. 确定子问题和状态

   既然我们知道动态规划是将母问题分解成诸多子问题，且保证每个子问题最优且无后效性（每个子问题的决策不能对后面其他未解决的问题产影响），那么我们得知道怎么分？刷完题后，发现问法相同或相似，**母问题和子问题区别只是规模变小了**。于是乎，我们可以尝试性从母问题减小规模，推出子问题。

   而状态则是子问题的解，而数组就是状态空间，而状态空间与时间复杂度是相关的，**整个问题的时间复杂度是状态数目乘以计算每个状态所需时间**。例如：硬币找零，总数为12元，我们就开dp[13]，对应每个下标就是一种状态，对于1元，我们会遍历每一种硬币，对于2元，我们也会遍历每一种硬币，以此类推便可知。

2. 转移方程

   转移方程，实际上就是**找母问题和子问题之间的联系，是核心，也是难点！**找到转移方程题目基本解完了，剩下只是边界问题。通过刷题，发现母问题大多数与前一个和或者两个有关，但是呢，转移方程又根据状态空间不同而略有改动。其实，我们只要把状态如何转移的递推关系搞明白，至于剩下的再仔细实现即可。

3. 初始条件和边界情况

   初始条件往往就是出发点，硬币找零求最少数量，给定的硬币对自身都算一枚，0元无硬币找，，这样我们就可以由前往后开始递推了。边界条件，**不要逾越数组或者出现负数下标。**

4. 计算顺序

   用已知的求未知。


> bilibili-九章算法课堂之动态规划



## 二、常见题型例题选讲

#### 1. 计数 -  lintcode 114 不同路径Ⅰ

题意：从左上角走到右下角共有多少条路径。

| （起点） |  1   |  1   |    1     |
| :------: | :--: | :--: | :------: |
|    1     |  2   |  3   |    4     |
|    1     |  3   |  6   |    10    |
|    1     |  4   |  10  | (终点)20 |



``` c++
class Solution {
public:
    /**
     * @param m: positive integer (1 <= m <= 100)
     * @param n: positive integer (1 <= n <= 100)
     * @return: An integer
     */
    int uniquePaths(int m, int n) {
        // write your code here
        if(m==0||n==0)return 0;//一条线
        int mp[n+1][m+1];//从（1，1）出发到（n，m）
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
            {
                if(i==1||j==1)mp[i][j]=1;//边界，只有一条路
                else mp[i][j]=mp[i-1][j]+mp[i][j-1];//到达(i,j)只有(i-1,j)和(i,j-1)左上两条路，直接相加即可
            }
        return mp[n][m];//返回终点值
    }
};
```



#### 2. 求最值 - 青蛙过河

题意：桥的长度为L，青蛙从位置0 跳到或跳过 位置L，就算跳出独木桥。[0,L]中间有n个石头，要求踩最少的石头过桥。

``` c++
//由于青蛙可以跳出桥即可，终点是不确定的，但是我们知道起点是0
//所以我们可以从后面往前面跳，只要最后一步跳到0这个位置就可以了
//在跳的步数【s,t】区间取最小的石头数目，dp【】状态数组记录最小的石头数量，当跳到0时候，踩到的石头一定是最小的

#include <iostream>
#include <algorithm>
#include <string.h>
#include <math.h>
#include <stdio.h>
using namespace std;

const int inf=0x3f3f3f;//定义无穷大
const int N=52;//石头数量
const int L=1e5+15;//桥的长度

int dp[L];//dp[]记录到达点L最少石头数

int main()
{
    int l,s,t,n,i,j;
    while(scanf("%d%d%d%d",&l,&s,&t,&n)!=EOF)
    {
        memset(dp,0,sizeof(dp));
        for(i=0; i<n; i++){
            int k;
            scanf("%d",&k);
            dp[k]=1;//根据题目，铺好石头
        }
        for(i=l; i>=0; --i){//从后往前跳，到起始点0
            int m=inf;
            for(j=s; j<=t; ++j)
                m=min(m,dp[i+j]);//找从起点i跳到 i+s -- i+t 最小石头数
            dp[i]+=m;//加上跳到i点最小的石头数量，代表i到桥末的最少石头数量
        }
        printf("%d\n",dp[0]);
    }
    return 0;
}
```



#### 3. 存在性 - 硬币找零

``` c++
#include <iostream>
#include <string.h>
#include <math.h>
#include <stdio.h>
using namespace std;

const int inf=1061109567;//定义无穷大——其实只要大于M即可
const int N=52;//硬币最大数
const int M=1e5+5;//找零最大数

int dp[M];//下标0~M是找零的总数，储存每次找零的最优解
int mony[N];//储存各种硬币

int main()
{
    int n,m,i,j;
    scanf("%d%d",&n,&m);//n是硬币系统中不同硬币数，m是需要用硬币找零的总数。
    memset(dp,inf,sizeof(dp));//初始化为正无穷，代表不存在用这些硬币可以找零
    for(i=0;i<n;i++){
        scanf("%d",&mony[i]);//储存各种硬币
        dp[mony[i]]=1;//每种硬币下dp[]肯定是1，一步到位嘛
    }
    dp[0]=0;//dp[0]无法计算，初始化为0
    for(i=0;i<=m;i++)
        for(j=0;j<n;j++)
    {
        //关键点！！
        //条件①：i-mony[j]>=0 如果小于0，说明当前的硬币无法找零，跳过，找下一个硬币
        //条件②：dp[i-mony[j]]!=inf 假设拿出该硬币mony[j]，那么剩余i-mony[j]的钱，而这钱不存在找零（inf表示不存在），则跳过
        if(i-mony[j]>=0&&dp[i-mony[j]]!=inf)
            dp[i]=min(dp[i-mony[j]]+1,dp[i]);//状态转移方程：假设取出该硬币和未取出该硬币比较，取最小找零的硬币数
    }
    if(dp[m]!=inf)printf("%d\n",dp[m]);//存在输出m找零的硬币数
    else printf("-1\n");//不存在输出-1
    return 0;
}
```





## 三、DAG上的DP

### 什么是DAG？

DAG是一种有向无环图，在连通的情况下，我们可以通过有向边进行状态转移，那么最值问题可以转化成路径最长或最短问题。

### DAG如何体现？

### 例题1：

#### 矩形嵌套

拿矩形嵌套此题为例。	

有n个矩形，每个矩形可以用a，b来描述，表示它的长和宽，如果一个矩形的长和宽**严格小于**另一个矩形的长和宽的时候，就说这个矩形可以嵌套在另一个矩形里（当然，a可以作为长，也可以作为宽），现在我们的问题是选出最多的矩形，输出**最多符合条件的矩形数目**。

![img](https://img-blog.csdn.net/20131016134531671?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHUxMDIwOTM1MjE5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



| x \ y |  1   |  2   |  3   |  4   |  5   |
| :---: | :--: | :--: | :--: | :--: | :--: |
| **1** |  0   |  1   |  0   |  1   |  1   |
| **2** |  0   |  0   |  0   |  0   |  0   |
| **3** |  1   |  1   |  0   |  1   |  1   |
| **4** |  0   |  0   |  0   |  0   |  0   |
| **5** |  0   |  0   |  0   |  0   |  0   |

**通过DAG构图，把问题转换为求最长（最短）路径问题。**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 1000 + 10;
int n,m;
int dp[maxn];                   //记录第i个矩形能嵌套的最大矩形数量
int mp[maxn][maxn];             //邻接矩阵建DAG

struct rectangle{				//结构体储存矩形信息
    int x,y;
    rectangle():x(0),y(0){}
};

int dp(int x)
{
    if(dp[x]!=-1)return dp[x];      //记忆化搜索
    int ans=1;
    for(int i=0;i<n;i++){
        if(mp[x][i]!=-1){           //有向边存在
            ans=max(ans,dp(i)+1);   //继续往里搜
        }
    }
    dp[x]=ans;//记录结果
    return dp[x];
}

int main()
{
    int t;
    scanf("%d",&t);
    while(t--){
        memset(dp,-1,sizeof(dp));                  //初始化
        memset(mp,-1,sizeof(mp));				   //初始化
        scanf("%d",&n);
        rectangle r[n];
        for(int i=0,a,b;i<n;i++)
        {
            //保证左小于右，a<b
            scanf("%d%d",&a,&b);
            if(a>b)swap(a,b);
            r[i].x=a;r[i].y=b;
        }

        for(int i=0;i<n;i++)                        //建立有向图
            for(int j=0;j<n;j++)
                if(r[i].x>r[j].x&&r[i].y>r[j].y)
                    mp[i][j]=1;						//表示第i个矩形和第j个矩形之间存在关系，i->j画一条有向边

        int ans=0;                                  //从每个点出发，找到最大的ans
        for(int i=0;i<n;i++)
            ans=max(ans,dp(i));
        printf("%d\n",ans);
    }
    return 0;
}

/*注释：感兴趣的可以用最长上升子序列做一下*/
```



### 例题2：

#### 硬币问题

有n种硬币，面值分别为V1 , V2 , …, Vn，每种都有无限多。给定非负整数S， 可以选用多少个硬币，使得面值之和恰好为S？输出硬币数目的最小值和最大值。 1≤n≤100，0≤S≤10000，1≤Vi≤S。

##### 思路

尽管跟上题矩形嵌套很不一样，但是本质也是DAG上的问题。为何？开始时，我们有S元，目的是0元，那是不是变成用 最多/少 的硬币从 S ---> 0 元？那么此问题就成了 “最短路和最长路” 问题了。

``` c++
#include<bits/stdc++.h>
using namespace std;

#define inf 0x3f3f3f3f
const int maxn=1e4+5;               //最大面额
const int maxm=1e3+5;               //最大硬币数
int dp1[maxn],dp2[maxn];             //dp[][]维护最少硬币数,dp1[][]维护最多硬币数
int f[maxm];                        //记录每个硬币面额
int main()
{
    int n,s;
    scanf("%d%d",&n,&s);
    memset(dp1,inf,sizeof(dp1));      //初始化
    memset(dp2,-inf,sizeof(dp2));   //初始化
    for(int i=0; i<n; i++)
        scanf("%d",&f[i]);
    dp1[0]=dp2[0]=0;;                        //0元需要0个硬币去凑
    for(int i=1; i<=s; i++)
    {
        for(int j=0; j<n; j++)
        {
            if(i>=f[j])
            {
                //面对每个硬币，选择取还是不取
                dp1[i]=min(dp1[i],dp1[i-f[j]]+1);      //核心代码
                dp2[i]=max(dp2[i],dp2[i-f[j]]+1);   //核心代码
            }
        }
    }
    printf("%d %d\n",dp1[s],dp2[s]);
    return 0;
}
```



#### 个人体会

问题呈现在DAG图上，可以很直观地感受状态是如何转移的。细想一下，即使我们不去构图，dp实质上一直存在“DAG”，我们正是要仔细揣摩问题之间的联系，找到子问题和母问题之间的线索，找到那根线，也就是有向边，**使问题小规模化，简单化**。而这根弦，我们总是叫它状态转移方程。做题来说，难找，但是找到的话，问题就基本解决了。



## 四、01背包

### 问题描述

有n个物品，它们有各自的体积和价值，现有给定容量的背包，如何让背包里装入的物品具有最大的价值总和？（通俗点，就是拿有限的袋子装更多的东西（仅一件），东西件数不同，背包问题种类不同）



### 总体思路

在解决问题之前，为描述方便，首先定义一些变量：**Vi表示第 i 个物品的价值，Wi表示第 i 个物品的重量，定义V(i,j)：当前背包容量 j，前 i 个物品最佳组合对应的价值 **。

1. 求背包装最大价值总和，即max（ΣVi）;

2. 约束条件，ΣWi <= capacity ;

3. 递推公式，面对物品只有两种可能。

   · Wi<now_capacity 

   · Wi>=now_capacity 

   由此我们可以写出递推公式

   · j<w(i)    V(i,j)=V(i-1,j)													不能装，则背包总价值等于前i-1背包的价值

   · j>=w(i)   V(i,j)=max｛V(i-1,j)，V(i-1,j-w(i))+v(i)｝      能装，但装或者不装，我们要最多的 (●'◡'●)！

4. 计算填表即可



#### 核心代码

``` c++
for(int i=1; i <= n; i++)
        for(int j=0; j <= Capacity; j++){
            if(j < w[i])//如果小于物品重量，则当前背包总价值等于前i-1个物品的背包最大总价值
                dp[i][j] = dp[i-1][j];
            else      //如果大于或等于物品重量，则在前i-1个物品的背包价值和当前装w[i]之后背包总价值取max
                dp[i][j] = max(dp[i-1][j],dp[i-1][j-w[i]]+v[i]);
        }

//	int dp[N][W];//表示前N个背包大小为W时候的最大总价值
//	int w[W],v[P];//w[]物品重量,v[]物品价值
```

#### 模拟过程

假设背包大小 Capacity 为12

| 序号i | 重量w | 价值v |
| :---: | :---: | :---: |
|   1   |   2   |   2   |
|   2   |   3   |   4   |
|   3   |   4   |   1   |
|   4   |   7   |   8   |

最初的时候每个状态的背包总价值都为0

| N \ C |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   1   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |
|   2   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |
|   3   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |
|   4   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |
|   5   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |  0   |

------

首先我们看物品 1 (w, v) = (2, 2) ， 显然，从Capacity >= 2开始都是可以装该物品的。而 C=0/1 装不了 

| N \ C |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   1   |  0   |  0   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |

------

接下来我们看物品 2 (w, v) = (3, 4)

因为C∈[0,2] < 3 ,故此时价值等于 前 i-1 背包的总价值 ，从C>=3 开始，我们需要考虑当前物品装还是不装，因为求的是最大背包种价值，使用我们要贪心些。对应上面的**递推公式**：

**· j<w(i)    V(i,j)=V(i-1,j)													不能装，则背包总价值等于前i-1背包的价值**

**· j>=w(i)   V(i,j)=max｛V(i-1,j)，V(i-1,j-w(i))+v(i)｝      能装，但装或者不装，我们要最多的 (●'◡'●)！**

​	 `dp[2][3] = max( dp[1][3] , dp[1][3-3] + v[2] )`   

=   `dp[2][3] = max( 2 , 0 + 4 )`  

=   `dp[2][2] = 4`

​	 `dp[2][4] = max( dp[1][4] , dp[1][4-3] + v[2] )`   

=   `dp[2][3] = max( 2 , 0 + 4 )`  

=   `dp[2][2] = 4`

​	 `dp[2][5] = max( dp[1][5] , dp[1][5-3] + v[2] )`   

=   `dp[2][3] = max( 2 , 2 + 4 )`  

=   `dp[2][2] = 6`

==滚动数组就算是再重复这样的事情直至全部遍历完毕==

| N \ C |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   1   |  0   |  0   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |
|   2   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |

------



接下我们看物品 3 (w, v) = (4, 1)

| N \ C |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   1   |  0   |  0   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |
|   2   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |
|   3   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  6   |  6   |  7   |  7   |  7   |  7   |

------

最后看物品 4 (w, v) = (7, 8)

| N \ C |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |  11  |  12  |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|   1   |  0   |  0   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |  2   |
|   2   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |  6   |
|   3   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  6   |  6   |  7   |  7   |  7   |  7   |
|   4   |  0   |  0   |  2   |  4   |  4   |  6   |  6   |  8   |  8   |  10  |  12  |  12  |  14  |

填表完毕，最后直接输出 ` dp[4][12]` 就可以了

#### 小结

为什么它符合01背包呢？因为我们每次只取其中一种背包，之后就再不理睬它了，之后的滚动数组也是如此，保证每种物品只会被取一次，又因为每种状态是符合最优解的，故我们得到的结果肯定也是最优解。



### 例题 51Nod - 1085---背包问题

![image-20210812230704905](C:\Users\12822\AppData\Roaming\Typora\typora-user-images\image-20210812230704905.png)

``` c++
//两种方案，空间复杂度不同，但时间复杂度相同
/*
空间未优化版本
*/
#include<iostream>
#include<string.h>

using namespace std;

const int W=1e4+5;//最大体积
const int P=1e4+5;//最大价值
const int N=105;//物品最多个数
int dp[N][W];//表示前N个背包大小为W时候的最大总价值
int w[W],v[P];//w[]物品重量,v[]物品价值
int main()
{
    int n,ww;//物品个数以及背包大小
    cin >> n >> ww;
    for(int i=1; i <= n; i++)//把第i个物品的体积以及价值保存到w[]和v[]数组里
        cin >> w[i] >> v[i];
        
    for(int i=1; i <= n; i++)
        for(int j=0; j <= ww; j++){
            if(j < w[i])//如果小于物品重量，则当前背包总价值等于前i-1个物品的背包最大总价值
                dp[i][j] = dp[i-1][j];
            else      //如果大于或等于物品重量，则在前i-1个物品的背包价值和当前装w[i]之后背包总价值取max
                dp[i][j] = max(dp[i-1][j],dp[i-1][j-w[i]]+v[i]);
        }
    cout << dp[n][ww] << endl;
    return 0;
}

/*

空间优化版
（1）把遍历第i个物体和遍历第i-1个物体时的最大价值存在一个单元里。更新前f[j]存i-1的价值，更新后f[j]存i的价值。因为用不到i-2及以前的数据所以不需要存。因为以后不会再用到i-1的价值所以被覆盖了没问题
（2）j从背包容量V开始遍历，即从大到小遍历，保证了当前f[j]和f[j - w[i]]里面存的是i-1的数据，即等价于f([i])[j] = max(f([i - 1])[j], f([i - 1])[j - w[i]] + v[i])，从而和优化空间复杂度前状态转移方程的原理一致。
（3）j从背包容量V开始遍历，如果与当前的v<w[i]不用继续比较，直接跳出循环

*/
#include<iostream>
#include<string.h>

using namespace std;

const int W=1e4+5;
const int P=1e4+5;
const int N=105;
int dp[W]; //一维数组储存，降低空间复杂度，表示容纳w重量时背包最大总价值
int w[W],v[P];
int main()
{
    int n,ww;
    cin >> n >> ww;
    for(int i=1; i<=n; i++)
        cin >> w[i] >> v[i];

    for(int i=1; i<=n; i++)
    {
        for(int j=ww; j>=w[i]; j--) // j<w[i]无法装包，直接跳出循环
        {
            dp[j]=max(dp[j],dp[j-w[i]]+v[i]);
        }
    }
    cout << dp[ww] <<endl;
    return 0;
}
```



### 参考资料

1. [算法入门系列二--DP入门之DAG上的DP_844604778-CSDN博客](https://blog.csdn.net/iteye_1485/article/details/82544514?ops_request_misc=%7B%22request%5Fid%22%3A%22162781328916780366548304%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=162781328916780366548304&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-82544514.first_rank_v2_pc_rank_v29&utm_term=dag上的dp&spm=1018.2226.3001.4187)

2. [教你彻底学会动态规划——入门篇_常敲代码手不抖-CSDN博客_动态规划](https://blog.csdn.net/baidu_28312631/article/details/47418773?ops_request_misc=%7B%22request%5Fid%22%3A%22162768336916780265434637%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=162768336916780265434637&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-4-47418773.first_rank_v2_pc_rank_v29&utm_term=动态规划&spm=1018.2226.3001.4187)

3. [动态规划解决01背包问题_Christal_RJ的博客-CSDN博客](https://blog.csdn.net/qq_39560607/article/details/81714194?ops_request_misc=%7B%22request%5Fid%22%3A%22162781910316780269883135%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=162781910316780269883135&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-81714194.first_rank_v2_pc_rank_v29&utm_term=动态规划01背包问题&spm=1018.2226.3001.4187)

4. [【动态规划专题班】ACM总冠军、清华+斯坦福大神带你入门动态规划算法_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1xb411e7ww?from=search&seid=2166045206164166653)

5. [动态规划理论：一篇文章带你彻底搞懂最优子结构、无后效性和重复子问题_every__day的博客-CSDN博客_动态规划无后效性](https://blog.csdn.net/every__day/article/details/88174082?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-2.baidujsUnder6&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-2.baidujsUnder6)

6. [Floyd算法的动态规划本质_紫芝的博客-CSDN博客](https://blog.csdn.net/qq_40507857/article/details/80657007?ops_request_misc=&request_id=&biz_id=102&utm_term=松弛操作本质&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187)

7. [动态规划与搜索递推的区别_baihehaitangyijiu的博客-CSDN博客](https://blog.csdn.net/baihehaitangyijiu/article/details/105503152)

8. [什么是无后效性？_涛声依旧的博客-CSDN博客_无后效性](https://blog.csdn.net/qq_30137611/article/details/77655707)

## 五、题单：

| 序号 | 题号                                 | 标题                       | 题型        | 难度 |                             题解                             |
| :--- | ------------------------------------ | -------------------------- | ----------- | ---- | :----------------------------------------------------------: |
| 1    | luogu - P1060                        | 开心的金明                 | 01背包      | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1060.cpp) |
| 2    | 51Nod - 1085                         | 背包问题                   | 01背包      | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/51Nod - 1085.cpp) |
| 3    | hdu - 2602                           | Bone Collector             | 01背包      | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/Bone Collector.md) |
| 4    | poj - 3624                           | Charm Bracelet             | 01背包      | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/poj - 3624 .cpp) |
| 5    | SCU - 4580                           | 动态规划之01背包问题       | 01背包      | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/SCU - 4580.cpp) |
| 6    | luogu - P1802                        | 5倍经验                    | 01背包      | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1802.cpp) |
| 7    | Liuser #725                          | 硬币问题                   | DAG - DP    | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/Liuser %23725.cpp) |
| 8    | POJ 1163                             | The Triangle               | DAG - DP    | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/POJ 1163 The Triangle.cpp) |
| 9    | 计蒜客 T1408                         | 矩形嵌套                   | DAG - DP    | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/计蒜客 T1408.cpp) |
| 10   | luogu - UVA116                       | 单向TSP Unidirectional TSP | DAG - DP    | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 UVA116.cpp) |
| 11   | Atcoder Beginner Contest 211 Task D  | Number of Shortest paths   | DAG - DP    | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/D - Number of Shortest paths.cpp) |
| 12   | UVA - 437                            | The Tower of Babylon       | DAG - DP    | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/UVA - 437.cpp) |
| 13   | lintcode 114                         | 不同路径 Ⅰ                 | DP - 计数   | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/lintcode 114.md) |
| 14   | hoj - 1186                           | 青蛙过河                   | DP - 计数   | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/哈工大 1186.cpp) |
| 15   | luogu - P1002                        | 过河卒                     | DP - 计数   | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1002.cpp) |
| 16   | AtCoder Beginner Contest 211 Task：C | chokudai                   | DP - 最值   | ⭐⭐   | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/Task C chokudai .cpp) |
| 17   | luogu - P1004                        | 方格取数                   | DP - 最值   | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1004.cpp) |
| 18   | luogu - P1282                        | 多米诺骨牌                 | DP - 最值   | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1282 多米诺骨牌.cpp) |
| 19   | luogu - P1435                        | 回文字串                   | DP - 最值   | ⭐⭐⭐  | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/洛谷 P1435.cpp) |
| 20   | 计蒜客 - T1781                       | 硬币找零                   | DP - 存在性 | ⭐    | [👍](https://gitee.com/ZhangYu_LOVE_MieMie/SZTU_ZhangYu_Solution/blob/main/solution/动态规划初步 & 01背包 & DAG上DP/计蒜客 - T1781.cpp) |



（集训题单）

1. [硬币找零 - 计蒜客 T1781 - Virtual Judge (vjudge.net)](https://vjudge.net/problem/计蒜客-T1781)
2. [青蛙过河 - HRBUST 1186 - Virtual Judge (vjudge.net)](https://vjudge.net/problem/HRBUST-1186)
3. [过河卒 - 计蒜客 T2118 - Virtual Judge (vjudge.net)](https://vjudge.net/problem/计蒜客-T2118)
4. [Bone Collector - HDU 2602 - Virtual Judge (vjudge.net)](https://vjudge.net/problem/HDU-2602)
5. [矩形嵌套 - 计蒜客 T1408 - Virtual Judge (vjudge.net)](https://vjudge.net/problem/计蒜客-T1408)

