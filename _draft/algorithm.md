---
layout: post
title: 算法
categoires: Develop
---

- [排序算法](#排序算法)
	- [比较排序](#比较排序)
		- [插入排序](#插入排序)
			- [直接插入排序](#直接插入排序)
			- [希尔排序](#希尔排序)
		- [选择排序](#选择排序)
			- [简单选择排序](#简单选择排序)
			- [堆排序](#堆排序)
		- [交换排序](#交换排序)
			- [冒泡排序](#冒泡排序)
			- [快速排序](#快速排序)
		- [分治排序（归并排序）](#分治排序归并排序)
	- [非比较排序](#非比较排序)
		- [基数排序](#基数排序)
		- [桶排序](#桶排序)
- [数据结构](#数据结构)
- [排列组合](#排列组合)

## 排序算法
### 比较排序
#### 插入排序
##### 直接插入排序

理解：把一张牌插入已经排好序的扑克牌
```
void InsertSort(int* data, int length) {
    for (int i = 1; i < length; i++) {
        if (data[i] < data[i-1]) {
            int sentry = data[i];
            int j = i-1;
            while (j >=0 && sentry < data[j]) {
                data[j+1] = data[j];
                j--;
            }
            data[j+1] = sentry;
        }
    }
}
```

##### 希尔排序

先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序。。
```
void ShellInsertSort(int* arr, int n, int gap) {
    for (int i = gap; i < n; i++) {
        if (arr[i] < arr[i-gap]) {
            int sentry = arr[i];
            int j = i-gap;
            while(j >= 0 && arr[j] > sentry) {
                arr[j+gap] = arr[j];
                j -= gap;
            }
            arr[j+gap] = sentry;
        }
    }
}

void ShellSort(int *arr, int n) {
    for (int gap = n/2; gap > 0; gap = gap/2) {
        ShellInsertSort(arr, n, gap);
    }
}
```

#### 选择排序

##### 简单选择排序

每次取最小值放在前面位置，选择n-1次。
```
int GetMinIndex(int* data, int start, int end) {
    int index = start;
    for (int i = start+1; i <= end; i++) {
        if (data[i] < data[index]) {
            index = i;
        }
    }
    return index;
}

void SelectSort(int* data, int length) {
    for (int i = 0; i < length-1; i++) {
        int index = GetMinIndex(data, i, length-1);
        Exchange(data[i], data[index]);
    }
}
```

##### 堆排序

1. 建立最大堆
2. 把堆顶和最后一个元素交换，使堆大小-1，调整堆
3. 循环length-1次
```
void AdjustMaxHeap(int* data, int length, int i) {
    int child = 2*i+1;
    if (child >= length) {
        return;
    }
    if (child+1 < length && data[child+1] > data[child]) {
        child++;
    }
    if (data[child] > data[i]) {
        Exchange(data[child], data[i]);
        AdjustMaxHeap(data, length, child);
    }
}

void AdjustMaxHeapOptimation(int* data, int length, int i) {
    while(i < length) {
        int child = 2*i+1;
        if (child >= length) {
            return;
        }
        if (child+1 < length && data[child+1] > data[child]) {
            child++;
        }
        if (data[child] > data[i]) {
            Exchange(data[child], data[i]);
            i = child;
        } else {
            break;
        }
    }
}

void BuildMaxHeap(int* data, int length) {
    for (int i = length/2-1; i >= 0; i--) {
        AdjustMaxHeapOptimation(data, length, i);
    }
}

void HeapSort(int* data, int length) {
    BuildMaxHeap(data, length);
    for (int i = length-1; i > 0; i--) {
        Exchange(data[0], data[i]);
        AdjustMaxHeapOptimation(data, i, 0);
    }
}
```

#### 交换排序

##### 冒泡排序

通过不断交换，把大数移到最后。
```
void Exchange(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

void BubbleSort(int* data, int length) {
    for (int i = 0; i < length; i++) {
        for (int j = 0; j < length-1-i; j++) {
            if (data[j] > data[j+1]) {
                Exchange(data[j], data[j+1]);
            }
        }
    }
}

```
##### 快速排序

以某个元素为标准，分成两组，不断递归。
```
int GetQuickSortIndex(int* data, int start, int end) {
    int sentry = data[start];
    while (start < end) {
        while (start < end && data[end] >= sentry) {
            end--;
        }
        data[start] = data[end];
        while(start < end && data[start] <= sentry) {
            start++;
        }
        data[end] = data[start];
    }
    data[start] = sentry;
    return start;
}

void QuickSortCore(int* data, int start, int end) {
    if (start >= end) {
        return;
    }
    int index = GetQuickSortIndex(data, start, end);
    QuickSortCore(data, start, index-1);
    QuickSortCore(data, index+1, end);
}

void QuickSort(int* data, int length) {
    QuickSortCore(data, 0, length-1);
}
```



#### 分治排序（归并排序）

理解： 把两堆已经排好序的扑克牌做merge
```
void Merge(int* from, int* to, int start, int middle, int end) {
    int i = middle;
    int j = end;
    int k = end-start;
    
    while(i >= 0 && j >= middle+1) {
        if (from[i] >= from[j]) {
            to[k--] = from[i--];
        } else {
            to[k--] = from[j--];
        }
    }
    
    while (i >= 0) {
        to[k--] = from[i--];
    }
    
    while (j >= middle+1) {
        to[k--] = from[j--];
    }
    
}
void MergeSort(int* data, int start, int end) {
    if (start == end) {
        return;
    }
    
    int middle = (start+end)/2;
    MergeSort(data, start, middle);
    MergeSort(data, middle+1, end);
    
    int* temp = new int [end-start+1];
    
    Merge(data, temp, start, middle, end);
    
    for (int i = start; i <= end; i++) {
        data[i] = temp[i-start];
    }
    
    delete []temp;
}
```


### 非比较排序

####1. 计数排序

用一个辅助数组保存第几大。
```
typedef bool(*Compare) (int a, int b);

bool Small(int a, int b) {
    if (a < b) {
        return true;
    }
    return false;
}

bool Big(int a, int b) {
    if (a > b) {
        return true;
    }
    return false;
}

int GetValue(int* data, int length, Compare compare) {
    int result = data[0];
    for (int i = 0; i < length; i++) {
        if (compare(data[i], result)) {
            result = data[i];
        }
    }
    return result;
}

void CountingSort(int* data, int length) {
    int max = GetValue(data, length, Big);
    int min = GetValue(data, length, Small);
    
    int* hash = new int [max-min+1];
    for (int i = 0; i < max-min+1; i++) {
        hash[i] = 0;
    }
    
    for (int i = 0; i < length; i++) {
        hash[data[i]-min]++;
    }
    
    for (int i = 1; i < max-min+1; i++) {
        hash[i] += hash[i-1];
    }
    
    int* dest = new int [length];
    for (int i = length-1; i >= 0; i--) {
        dest[hash[data[i]-min]-1] = data[i];
        hash[data[i]-min]--;
    }
    
    for (int i = 0; i < length; i++) {
        data[i] = dest[i];
    }
    
    delete [] hash;
    delete [] dest;
}
```

#### 基数排序

分解成d位数，每位采用合适的排序方法(如计数排序), 由低位到高位连续排。

```
Radix-Sort(A, d)
	use a stable sort to sort arry A on digit i
```

#### 桶排序

使用于数据位于[0,1), 将数据插入n个桶（类似计数排序），然后在对桶进行比较排序。


## 数据结构

基本：栈，队列，链表，树

散列表：k到h(k)的映射，h(k)散列函数

二叉搜索树: left < root < right, 中序，左序，右序遍历

红黑树：一种平衡二叉树

## 排列组合

