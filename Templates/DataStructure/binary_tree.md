## 二叉树

### 二叉树非递归遍历

层次遍历

```cpp
void LevelOrder()
{
    queue<int> q;
    q.push(0);
    while(!q.empty())
    {
        int now = q.front();
        q.pop();
        if(tr[now].v == '0')
            continue;
        printf("%c", tr[now].v);
        if(tr[now].l != -1) q.push(tr[now].l);
        if(tr[now].r != -1) q.push(tr[now].r);
    }
    printf("\n");
}
```

先序遍历

```cpp
void PreOrderNonRecursive()
{
    stack<int> stNode;
    int now = 0;
    if(tr[now].v == '#') return;  // '#' 表示空子树
    while(!stNode.empty() || tr[now].v != '#')
    {
        if(tr[now].v != '#')
        {
            printf("%c", tr[now].v);
            stNode.push(now);
            now = tr[now].l;
        }
        else if(!stNode.empty())
        {
            now = stNode.top();
            stNode.pop();
            now = tr[now].r;
        }
    }
    printf("\n");
}
```

中序遍历

```cpp
void InOrderNonRecursive()
{
    stack<int> stNode;
    int now = 0;
    if(tr[now].v == '#') return;
    while(!stNode.empty() || tr[now].v != '#')
    {
        if(tr[now].v != '#')
        {
            stNode.push(now);
            now = tr[now].l;
        }
        else if(!stNode.empty())
        {
            now = stNode.top();
            stNode.pop();
            printf("%c", tr[now].v);
            now = tr[now].r;
        }
    }
    printf("\n");
}
```

后序遍历

```cpp
void PostOrderNonRecursive()
{
    stack<int> stNode, stStatus;
    int now = 0;
    if(tr[now].v == '#') return;
    while (!stNode.empty() || now != -1 && tr[now].v != '#')
    {
        if(now == -1 || tr[now].v == '#')
        {  // 遇空指针（空树）出栈
            now = stNode.top();
            if(stStatus.top() == 0)  
            {  // 0 标记要往右去
                now = tr[now].r;
                stStatus.pop();
                stStatus.push(1);
            }
            else
            {  // 1 标记要返回（输出并出栈）
                printf("%c", tr[now].v);
                stNode.pop();
                stStatus.pop();
                now = -1;
            }
        }
        else
        {  // 非空则入栈，往左去
            stNode.push(now);
            stStatus.push(0);
            now = tr[now].l;
        }
    } 
    printf("\n");
}
```

### 哈夫曼编码

给定字符串求哈夫曼编码长度

```cpp
const int MAXCODE = 128;
int cnt[MAXCODE];
int GetHuffmanLength(char st[])
{
    int length = 0;
    priority_queue<int, vector<int>, greater<int> > q;
    memset(cnt, 0, sizeof(cnt));
    for(int i = 0; st[i]; i ++)
        cnt[st[i]] ++;
    for(int i = 0; i < MAXCODE; i ++)
        if(cnt[i]) q.push(cnt[i]);
    for(length = 0; q.size() > 1; )
    {
        int left = q.top(); q.pop();
        int right = q.top(); q.pop();
        length += left + right;
        q.push(left + right);
    }
    if(length == 0 && !q.empty())
        length = q.top();
    return length;
}
```

给定权值序列求赫夫曼树带权路径和

```cpp
LL GetHuffmanSum(int w[], int wLen)
{
    LL length = 0;
    priority_queue<LL, vector<LL>, greater<LL> > q;
    for(int i = 0; i < wLen; i ++)
        q.push(w[i]);
    for(length = 0; q.size() > 1; )
    {
        int left = q.top(); q.pop();
        int right = q.top(); q.pop();
        length += left + right;
        q.push(left + right);
    }
    return length;
}
```

## 平衡树

### AVL

```cpp
// 以POJ3481为例，插入 data 与 key，以 key 为排序依据
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
const int maxn = 1e5 + 10;
struct Node
{
    int key;
    int data;
    int left;
    int right;
    int height;
    void Init(){data = key = left = right = height = -1;}
    void Init(int k_, int d_=0){Init(); key = k_; data = d_;}
    Node(){Init();}
    Node(int k_, int d_=0){Init(k_, d_);}
};

Node tr[maxn];
int root, tp; // root 为可变的根，tp全局内存指针，都初始化为-1

inline int UpdateHeight(int now)
{
    int lh = tr[now].left == -1 ? 0 : tr[tr[now].left].height;
    int rh = tr[now].right == -1 ? 0 : tr[tr[now].right].height;
    return tr[now].height = (lh > rh ? lh : rh) + 1;
}
inline int HeightDiff(int now)
{
    int lh = tr[now].left == -1 ? 0 : tr[tr[now].left].height;
    int rh = tr[now].right == -1 ? 0 : tr[tr[now].right].height;
    return lh - rh;
}
int LL(int an)
{
    int bn = tr[an].left;
    int dn = tr[bn].right;
    tr[bn].right = an;
    tr[an].left = dn;
    UpdateHeight(an);
    UpdateHeight(bn);
    return bn;
}
int RR(int an)
{
    int bn = tr[an].right;
    int dn = tr[bn].left;
    tr[bn].left = an;
    tr[an].right = dn;
    UpdateHeight(an);
    UpdateHeight(bn);
    return bn;
}
int LR(int an)
{
    tr[an].left = RR(tr[an].left);
    return LL(an);
}
int RL(int an)
{
    tr[an].right = LL(tr[an].right);
    return RR(an);
}
void Insert(int &now, int key, int data=0)
{
    if(now == -1)
    {
        now = ++ tp;
        tr[now].Init(key, data);
    }
    else if(key < tr[now].key)
    {
        Insert(tr[now].left, key, data);
        if(HeightDiff(now) == 2)
            now = key < tr[tr[now].left].key ? LL(now) : LR(now);
    }
    else if(key > tr[now].key)
    {
        Insert(tr[now].right, key, data);
        if(HeightDiff(now) == -2)
            now = key > tr[tr[now].right].key ? RR(now) : RL(now);
    }
    UpdateHeight(now);
}
void Delete(int &now, int key)
{
    if(now == -1) return;
    else if(key < tr[now].key) Delete(tr[now].left, key);
    else if(key > tr[now].key) Delete(tr[now].right, key);
    else
    {
        if(tr[now].left == -1) now = tr[now].right;
        else if(tr[now].right == -1) now = tr[now].left;
        else
        {
            int nd;
            for(nd = tr[now].left; tr[nd].right != -1; nd = tr[nd].right);
            tr[now].key = tr[nd].key; tr[now].data = tr[nd].data;
            Delete(tr[now].left, tr[nd].key);
            if(HeightDiff(now) == 2)
                now = HeightDiff(tr[now].left) == 1 ? LL(now) : LR(now);
            else if(HeightDiff(now) == -2)
                now = HeightDiff(tr[now].right) == -1 ? RR(now) : RL(now);
            UpdateHeight(now);
        }
    }
}

int GetFirst(int now)
{
    if(now == -1) return now;
    for(; tr[now].left != -1; now = tr[now].left);
    return now;
}
int GetLast(int now)
{
    if(now == -1) return now;
    for(; tr[now].right != -1; now = tr[now].right);
    return now;
}

int main()
{
    int op, key, data;
    for(root = tp = -1; scanf("%d", &op) && op; )
    {
        if(op == 1)
        {
            scanf("%d%d", &data, &key);
            Insert(root, key, data);
        }
        else if(op == 2)
        {
            int ith = GetLast(root);
            printf("%d\n", ith == -1 ? 0 : tr[ith].data);
            if(ith != -1) Delete(root, tr[ith].key);
        }

        else if(op == 3)
        {
            int ith = GetFirst(root);
            printf("%d\n", ith == -1 ? 0 : tr[ith].data);
            if(ith != -1) Delete(root, tr[ith].key);
        }
        else
            break;
    }
    return 0;
}
```
