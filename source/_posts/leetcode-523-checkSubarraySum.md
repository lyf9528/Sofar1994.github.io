---
title: leetcode-523-checkSubarraySum
author: Sofar
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-02 11:19:35
coverImg:
password:
summary: 同余定理：如果两个整数m、n满足n-m能被k整除，那么n和m对k同余
tags:
- leetcode
- 哈希表
- 前缀和
- 同余
categories:
---


```go

func checkSubarraySum(nums []int, k int) bool {
    numLen := len(nums)
    if numLen < 2 {
        return false
    }
    preSum := map[int]int{0:-1}
    remainder := 0
    for i, num := range nums {
       remainder = (remainder + num) % k
       if preIndex, ok := preSum[remainder]; ok {
           if i - preIndex >= 2 {
               return true
           }
       } else {
           preSum[remainder] = i
       }
    }
    return false
}
```