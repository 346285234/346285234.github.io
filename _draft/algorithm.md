---
layout: post
title: 算法导论
categoires: Develop
---



left question：递归与非递归方法？





实际问题：给你一组股票数据，什么时候买入什么时候卖出收益最大

抽象：最大子数组问题

解决方案：

1. 暴力破解Θ(n^2)
2. 分治O(nlgn) - 分成两边，最大子数组一定是max(左边数组，右边数组，左边一个右边一个组成的数组)，因为左边最大子数组和右边最大子数组为原问题的递归子问题，左边一个右边一个=从中间到左边最大子数组+从中间到右边最大子数组=>T(n) = 2*T(n/2) + Θ(n) 

3. 找规律Θ(n) - left=0, right=1，right递增，计算left到right子数组和，记录最大值，如果当right=i时，sum<0，使left=i+1，right=i+2重复，直到遍历结束



实际问题：矩阵乘法

抽象：矩阵乘法

解决方案：

1. 暴力O(n^3)
2. 分治-将A,B,C三个n*n矩阵分别拆成A11,A12,A21,A22等4个n/2 * n/2矩阵，所以有T(n)=T(1)+8T(n/2)+O(n^2)
3. Strassen O(n^lg7) - 使S1=B12-B22...S10=B11+B12,使P1=A11*S1...P7=S9 * S10, 用P表示C，有T(n)=7T(n/2)+O(n^2)



证明：

T(n) = 8T(n/2) + Θ(n^2) =>T(n) = Θ(n^3)

T(n)=7T(n/2)+O(n^2)=>T(n)=O(n^lg7)

方法：

1. 做递归树
2. T(n)=等比数列和+所有叶子结点消耗



## 排序算法
### 比较排序

#### 插入排序

理解：把一张牌插入已经排好序的扑克牌

#### 分治排序

理解： 把两堆已经排好序的扑克牌做merge

#### 堆排序

O(nlgn) (最大堆（parent>=children），最小堆(parent<=children))
```
heapsort(A) {
	Build_max_heap(A) // 构建最大堆
	for i = A.length downto 2 {
		exchange A[1] with A[i]
		A.heapsize = A.heapsize - 1
		Max_heapify(A, 1) // 根结点逐级下降，使树重新成为最大堆
	}
}
```

#### 快速排序

O(nlgn) 最坏情况O(n^2)
```
quickSort(A, p, r)
	if p < r
		q = partition(A, p, r)
		quickSort(A, p, q-1)
		quickSort(A, q+1, r)
		
partition(A, p, r)
	x = A[r]
	i = p-1
	for j = p to r-1
		if A[j]<=x
			i=i+1
			exchange A[i] with A[j]
	exchange A[i+1] with A[r]
	return i+1
```

###计数排序

O(n)

```
counting-sort(A, B, k)
	let C[0..k] be a new array // c temp array
	for i=0 to k
		C[i] = 0
	for j=1 to A.length
		C[A[j]] = C[A[j]] + 1
	for i=1 to k
		c[i] += c[i-1]
	for j=A.length downto 1
		B[C[A[j]]] = A[j]
		C[A[j]] -= 1
```

### 基数排序

分解成d位数，每位采用合适的排序方法(如计数排序)

```
Radix-Sort(A, d)
	use a stable sort to sort arry A on digit i
```

## 数据结构
