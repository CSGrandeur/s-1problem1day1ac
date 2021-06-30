## FFT

### 前置需求1——自定义复数类
无需太复杂，只需实现加减乘运算即可。

`std::complex`在实际FFT使用过程中易出现常数过大的情况，所以需要自己实现一个。
```cpp
struct cpx {
    double r, i;
    cpx(double a = 0.0, double b = 0.0): r(a), i(b) {}
    cpx operator+(const cpx& tmp) {
        return cpx(r + tmp.r, i + tmp.i);
    }
    cpx operator-(const cpx& tmp) {
        return cpx(r - tmp.r, i - tmp.i);
    }
    cpx operator*(const cpx& tmp) {
        return cpx(r * tmp.r - i * tmp.i, r * tmp.i + i * tmp.r);
    }
};
```

### 前置需求2——二进制倒位算法
用递推即可在`O(n)`时间内完成。
```cpp
// 要求n为2的倍数，len为n的位数-1
int n, len;
int rvs[n];
for (int i = 1; i < n; ++i)
    rvs[i] = (rvs[i >> 1] >> 1) | ((i & 1) << (len - 1));
```

### 非递归FFT
FFT要求元素数量为2的倍数，所以n为2的倍数，len为n位数-1，op用于决定是进行FFT还是IFFT。
```cpp
// FFT(op == true) or IFFT(op == false)
void FFT(cpx* A, int n, int len, bool op) {
    // Reverse
    for (int i = 0; i < n; ++i)
        if (i < rvs[i])
            std::swap(A[i], A[rvs[i]]);
    // Start calculating
    for (int i = 1; i < n; i <<= 1) {
		    // FFT need sin(PI / i), but IFFT need -sin(PI / i)
		    cpx wn(cos(PI / i), sin(PI / i) * (op ? 1.0 : -1.0));
        for (int j = 0; j < n; j += (i << 1)) {
            cpx w(1.0, 0.0);
            for (int k = 0; k < i; ++k, w = w * wn) {
                cpx x = A[j + k], y = w * A[i + j + k];
                A[j + k] = x + y;
                A[i + j + k] = x - y;
            }
        }
    }
    // If is IFFT
    if (!op)
        for (int i = 0; i < n; ++i)
            A[i].r /= n;
}
```
