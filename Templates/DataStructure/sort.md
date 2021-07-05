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

### 堆排序

```c
void AdjustHeap(int arr[], int i, int len)
{
    int temp = arr[i];
    for (int j = i * 2 + 1; j < len; j = j * 2 + 1)
    {
        if (j + 1 < len && arr[j] < arr[j + 1])
            ++j;

        if (arr[j] > temp)
        {
            arr[i] = arr[j];
            i = j;
        }
        else
            break;
    }
    arr[i] = temp;
}

void HeapSort(int arr[], int len)
{
    for (int i = len / 2 - 1; i >= 0; --i)
        AdjustHeap(arr, i, len);

    for (int j = len - 1; j > 0; --j)
    {
        int temp = arr[j];
        arr[j] = arr[0];
        arr[0] = temp;
        AdjustHeap(arr, 0, j);
    }
}
```

### 计数排序

```c
void CountSort(int arr[], int len)
{
    if (len < 2)
        return;

    int max = INT_MIN;
    int min = INT_MAX;
    for (int i = 0; i < len; ++i)
    {
        if (arr[i] > max)
            max = arr[i];
        if (arr[i] < min)
            min = arr[i];
    }
    if (max == min)
        return;

    int countlen = max - min + 1;
    int *count = (int *)calloc(countlen, sizeof(int));
    memset(count, 0, sizeof(int) * countlen);

    for (int i = 0; i < len; ++i)
        ++count[arr[i] - min];
    for (int i = 0, index = 0; i < countlen; ++i)
    {
        while (count[i]--)
            arr[index++] = i + min;
    }
    free(count);
}
```

