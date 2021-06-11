---
title: leetcode-15 ThreeSum
author: Sofar
top: true
cover: false
toc: true
mathjax: true
date: 2021-06-01 18:32:58
coverImg:
password:
summary:
tags:
- leetCode
- 双指针
categories:
---

## 思路
外层循环 指针i遍历数组
内层循环 利用双指针获取三数之和 == 0 的元素

## 排序
有序数组有利于去重

## 双指针
指针 `l` = i + 1

指针 `r` = len(nums) - 1

在 `l` 指针小于 `r` 指针的前提下，如果 `nums[l] + nums[r] > nums[i]` ，需要 `右指针` 左移，并且要避免重复


```go

func threeSum(nums []int) [][]int {
    sort.Ints(nums) 
	//fmt.Println(nums)
	result := make([][]int,0)
	n := len(nums)
    // n - 2 
	for i := 0; i < n - 2; i++ {
		if nums[i] > 0{
			break
		}

		if i > 0 && nums[i] == nums[i-1] {
			continue
		}

		r := n - 1
		target := -1 * nums[i]

		for l := i + 1; l < r; l++{
			if l > i+1 && nums[l] == nums[l-1] {
				continue
			}
			for l < r && nums[l] + nums[r] > target {
				r--
			}
            // 指针重合 结束循环
			if l == r {
				break
			}
			if nums[l] + nums[r] == target {
				result = append(result,[]int{nums[i],nums[l],nums[r]})
			}
		}
	}
	return result
}

```
