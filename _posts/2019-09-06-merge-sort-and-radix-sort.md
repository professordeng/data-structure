---
title: 7.5 merge sort and radix sort
date: 2019-09-12
---

## 1. 归并排序

归并排序与基于交换、选择等排序的思想不一样，“归并” 的含义是将两个或两个以上的有序表组合成一个新的有序表。假定待排序表含有 n 个记录，则可将其视为 n 个有序的子表，每个子表的长度为 1，然后两两归并，得到 `n/2上取整` 个长度为 2 或 1 的有序表；再两两归并……如此重复，直到合并成一个长度为 n 的有序表为止，这种排序方法称为 2 路归并排序。

`merge()` 的功能是将前后相邻的两个有序表归并为一个有序表。设两段有序表 `L[low~mid]` 、`L[mid+1~high]` 存放在同一顺序表的相邻位置，先将它们复制到辅助数组 B 中。每次从对应 B 中的两个段取出一个记录进行关键字的比较，将较小者放入 A 中，当数组 B 中有一段的下标超出其对应的表长（即该段的所有元素已完全复制到 A 中）时，将另一段中的剩余部分直接复制到 A 中。

注意：代码中最后两个 while 循环只有一个会执行。

一趟归并排序的操作是，调用 `n/2h上取整` 次算法 `merge()` ，将 `L[1~n]` 中前后相邻且长度为 h 的有序段进行两两归并，得到前后相邻、长度为 2h 的有序段，整个归并排序需要进行 `log2n` 趟。

递归形式的 2 路归并排序算法是基于分治的，其过程如下：

分解：将含有 n 个元素的待排序表分成各含 n/2 个元素的子表，采用 2 路归并算法对这两个子表递归地进行排序。

合并：合并两个已排好序的子表得到排序结果。

2 路归并排序算法的性能分析如下：

空间效率：`merge()` 操作中，辅助空间刚好占 n 个单元，所以归并排序的空间复杂度为 `O(n)` 。

时间效率：每趟归并的时间复杂度为 `O(n)` ，共需进行 `log2n上取整` 趟排序，所以算法的时间复杂度为 `O(nlog2n)` 。

稳定性：由于 `merge()` 操作不会改变相同关键字记录的相对次序，所以 2 路归并排序算法是一种稳定的排序算法。

注意：一般而言，对于 N 个元素进行 k 路归并排序时，排序的趟数 m 满足 k^m=N，从而 `m=logkN` 。这和前面的 2 路归并是一致的 。 

## 2. 基数排序

基数排序是一种很特别的排序方法，它不基于比较进行排序，而采用多关键字排序思想（即基于关键字各位的大小进行排序），借助 “分配” 和 “收集” 两种操作对单逻辑关键字进行排序。基数排序又分为最高位优先（MSD）排序和最低位优先（LSD）排序。

以 r 为基数的最低位优先基数排序的过程：假设线性表由结点序列 `a0,a1,...,a(n-1)` 构成，每个结点 `aj` 的关键字由 d 元组 `(kj(d-1),kj(d-2),...,kj(1),kj(0)` 组成，其中 `0≤kj(i)≤r-1(0≤j<n,0≤i≤d-1)` 。在排序过程中，使用 `r` 个队列 `Q0,Q2,...,Q(R-1)` 。排序过程如下

对 `i=0,1,...,d-1` ，依次做一个 “分配” 和 “收集” （其实是一次稳定的排序过程）

分配：开始时，把各个队列清空，然后依次考察线性表中的每个结点 `aj(j=0,1,...,n-1)` ，若 `aj` 的关键字 `kj(i)=k` ，就把 `aj` 放进 `Qk` 队列中。

收集：把各个队列中的结点依次首尾相接，得到新的结点序列，从而组成新的线性表。

基数排序算法的性能分析如下：

空间效率：一趟排序需要的辅助存储空间为 r（r 个队列），但以后的排序中会重复使用这些队列，所以基数排序的空间复杂度为 `O(r)` 。

时间效率：基数排序需要进行 d 趟分配和收集，一趟分配需要 `O(n)` ，一趟收集需要 `O(r)` ，所以基数排序的时间复杂度为 `O(d(n+r))` ，它与序列的初始状态无关。

稳定性：对于基数排序算法而言，很重要的一点就是按位排序时必须是稳定的，因此，这也保证了基数排序的稳定性。