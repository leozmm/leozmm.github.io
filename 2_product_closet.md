# Closet produce pair in an array

> 原文链接： https://www.geeksforgeeks.org/closest-product-pair-array/

## 题目描述

给定一个数 x ， 和一个非负整数数组 arr ， 求出 arr 中积最接近 x 的那一对 （pair）数。

> 示例：
> 输入： arr[] = [2, 3, 5, 9]， x = 47
> 输出： (5, 9)

## 朴素解法（暴力解法）

两层循环，挨个比较所有数的两两组合的积，找出最小的。时间复杂度 $O(N^2)$。

## 优化1

1. 将数组按升序排序；
2. 定义变量 diff ，并初始化为无穷大，用以表示 pair 的积和 x 之间的差值，找到最小差值的 pair 即为答案；
3. 遍历排序后的 arr，进行如下操作：

    - 计算 $p = x / arr[i]$；
    - 在 arr[i + 1, n - 1] 中，找出不大于 p 的最大整数 low；
    - 在 arr[i + 1, n - 1] 中，找出不小于 p 的最小整数 up；
    - 计算 $tmp_diff = min(low, up)$，与 diff 值对比，如果 $tmp_diff < diff$， 则更新 diff = tmp_diff， 以及对应的 pair 数对。

该算法的时间复杂度：遍历需要 $O(N)$ 时间，每一次遍历时，寻找对应的 low 和 up， 可以用二分查找，需要 $O(lgN)$ 时间，合起来是 $O(N · lgN)$

## 优化2

将寻找 pair 的过程由 $O(N · lgN)$ 降到 $O(N)$ 。
1. 将数组排序；
2. 定义变量 diff ，并初始化为无穷大，用以表示 pair 的积和 x 之间的差值，找到最小差值的 pair 即为答案；
3. 定义两个索引：low = 0, hi = n - 1；
4. 遍历数组 arr，从数组两端夹逼。 while low < hi，执行：

    - if abs(arr[low] * arr[hi] - x) < diff， 更新 diff 值和 pair 对；
    - if arr[low] * arr[hi] < x， 则表明 arr[low] 不够大，low++；（因为 arr[hi] 是从最大的 arr[n - 1] 一路降下来的，所以发现两者乘积不够大的时候，只能增加 low，否则就是回溯到之前剪枝的部分了）；
    - else if arr[low] * arr[hi] > x， 则表明 arr[hi] 不够小，hi--;

1. 输出 pair 。

该算法中，除了排序部分，在寻找 pair 对的部分里，循环从数组两端开始，逐步向中间推进，所以时间复杂度为 $O(N)$ 。


> 类似的问题还有如“2 sum closet” 和 “3 sum closet” 等。