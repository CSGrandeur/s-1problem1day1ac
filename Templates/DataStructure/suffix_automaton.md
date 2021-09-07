# 后缀自动机

```cpp
// 简便宏
#define fill(pnt, val) memset(pnt, val, sizeof(pnt))
#define copy(from, to) memcpy(to, from, sizeof(to))

struct SuffixAutomaton {
    // 节点结构体
    struct Node {
        int next[26]; // 子节点
        int len, fa;  // 节点串长与父节点
        // 构造函数
        Node(): len(0) {
            fill(next, 0);
        }
        // 强赋值运算符重载
        Node& operator=(const Node& tmp) {
            copy(tmp.next, next);
            len = tmp.len;
            fa = tmp.fa;
            return *this;
        }
    } *T;
    int las, tot; // 上次操作的尾节点与总节点计数
    // 构造函数
    SuffixAutomaton(int sz): T(new Node[sz << 1]), las(1), tot(1) {}
    // 析构函数
    ~SuffixAutomaton() {delete[] T;}
    // 插入节点
    void ins(int c) {
        int p = las, np = ++tot;  // 记录尾节点并建立新节点
        las = tot;  // 更新下一次插入的尾节点
        T[np].len = T[p].len + 1; // 更新新节点的串长
        T[np].fa = p; // 更新新节点的父节点
        for (; p && !T[p].next[c]; p = T[p].fa) // 回溯parent树链，只要有空儿子就连到新节点，否则停止
            T[p].next[c] = np;
        if (!p) // 回到了根节点，那么新节点的父节点就是根节点
            T[np].fa = 1;
        else {  // 在非根节点停下
            int q = T[p].next[c];
            if (T[q].len == T[p].len + 1) // 检查非空儿子的串长是否为非根节点的串长+1，即可以构成子后缀的情况
                T[np].fa = q; // 新节点的父节点就是该非空儿子
            else {  // 不满足条件，将非空儿子进行点分割
                // 分出一个节点，并继承非空儿子的信息，然后对一些参数进行调整
                int nq = ++tot;
                T[nq] = T[q];
                T[nq].len = T[p].len + 1; // 假设它可以构成子后缀
                ///////////////////////////////////////////////////////
                T[np].fa = T[q].fa = nq;  // 调整非空儿子和新节点的父节点
                for (; T[p].next[c] == q; p = T[p].fa)  // 把所有指向非空儿子的后继指针调整到分出的新节点上
                    T[p].next[c] = nq;
            }
        }
    }
    // 使用一个只含小写字母的字符串来构造后缀自动机
    void build(const char* str) {
        for (int i = 0; str[i]; ++i)
            ins(str[i] - 'a');
    }
};
```
