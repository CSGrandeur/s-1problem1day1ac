## 排序

### 归并排序

```cpp
int tmp[maxn];
MergeSort(int a[], int left, int right)
{
    if(left >= right - 1) return 0;
    int mid = left + right >> 1;
    MergeSort(a, left, mid);
    MergeSort(a, mid, right);
    int i, j, k;
    for(i = k = left, j = mid; i < mid && j < right;)
    {
        if(a[i] <= a[j]) tmp[k ++] = a[i ++];
        else tmp[k ++] = a[j ++];
    }
    while(i < mid) tmp[k ++] = a[i ++];
    while(j < right) tmp[k ++] = a[j ++];
    memcpy(a + left, tmp + left, sizeof(a[0]) * (right - left));
    return ret;
}
```

### 快速排序

```cpp
void QuickSort(int a[], int left, int right)
{
    if(left >= right - 1) return;
    int low = left, high = right - 1, center = a[low];
    while(low < high)
    {
        while(low < high && a[high] >= center) high --;
        a[low] = a[high];
        while(low < high && a[low] <= center) low ++;
        a[high] = a[low];
    }
    a[low] = center;
    QuickSort(a, left, low);
    QuickSort(a, low + 1, right);
}
```
