---
title: 303-区域和检索
author: Sofar
date: 2021-06-02 13:17:59
tags:
- 前缀和
- 哈希表
categories: 算法
---
## 思路
nums数组的每一项都有对应的前缀和: nums的第0项到当前项的和。

用数组 `preSum`表示，preSum[i]: nums的第0项到第i项的和。

`nums的某项` = 两个相邻的和前缀和的差：
    
    nums[i] = preSum[i] - preSum[i-1]

而 `nums的某项的前缀和` 根据上述表达式转换得：   

    preSum[i] = preSum[i-1] + nums[i]

对于 nums的 i 到 j 的元素和，根据上边的式子可得：

    nums[i]+...+nums[j] = preSum[j] - preSum[i-1]

当 i 为0时，下标变成-1， 得出 preSum[-1] == 0  

preSum数组根据上述式子 `preSum[i] = preSum[i-1] + nums[i]`, 遍历nums得出

```go

type NumArray struct {
   numsLen int
   preSum  []int
}

func Constructor(nums []int) NumArray {
    if len(nums) == 0 {
		return NumArray{
            numsLen: 0,
            preSum: []int{},
        }
	}
	nA := NumArray{
		numsLen: len(nums),
		preSum:  make([]int, len(nums)),
	}
	nA.preSum[0] = nums[0]
	for i := 1; i < nA.numsLen; i++ {
		nA.preSum[i] = nA.preSum[i-1] + nums[i]
	}
	return nA
}

func (this *NumArray) SumRange(left int, right int) int {
    if left == 0 {
        return this.preSum[right]
    }
    return this.preSum[right] - this.preSum[left-1]
}

```

## 思路2

nums数组的每一项都有对应的前缀和: nums的第0项到当前项的和。

用数组 `preSum`表示，preSum[i]: nums的第0项到第i项的和。

    preSum[i] = nums[0]...+num[i-1]

得出

    preSum[i+1] = nums[0] + nums[1] ... + num[i] = preSum[i] + num[i]

则

    nums[i] = preSum[i+1] - preSum[i]

    nums[i] + ... + nums[j] = preSum[j+1] - preSum[i]

即

    sumRange(i,j) = preSum[j+1] - preSum[i]

```go

type NumArray struct {
   numsLen int
   preSum  []int
}

func Constructor(nums []int) NumArray {
    if len(nums) == 0 {
		return NumArray{
            numsLen: 0,
            preSum: []int{},
        }
	}
	nA := NumArray{
		numsLen: len(nums),
		preSum:  make([]int, len(nums)),
	}
	nA.preSum[0] = 0
	for i := 0; i < nA.numsLen; i++ {
		nA.preSum[i+1] = nA.preSum[i] + nums[i]
	}
	return nA
}

func (this *NumArray) SumRange(left int, right int) int {

    return this.preSum[right+1] - this.preSum[left]
}


```

