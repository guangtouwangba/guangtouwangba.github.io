---
layout:   page
title:       "动态规划 - 打家劫舍系列"
subtitle:    ""
description: ""
date:        2024-07-04
author:   "aiqiu"
categories:  ["Leetcode"]
published: true
---
# 动态规划 - 打家劫舍系列
## 打家劫舍

### 题目

一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响小偷偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组 nums ，请计算 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```bash
示例 1：
输入：nums = [1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：
输入：nums = [2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

### 分析
首先，这个题目求的是一个最高金额，一般求极值我们可以想到的是贪心算法以及dp。
另外，这道题目还有一个限制就是，要求不触发警报装置，也就是不能选取相邻的两个元素。
使用dp的思路来解决这道问题的话，假设现在的nums是`[1,2,3,1]​`,  最后一个元素是1，我们需要考虑是否选1能够偷到最高金额，
* 如果选了1，则是 `dp[2]+1` (因为我们不能选择相邻的两个元素，那么必然是需要选择`dp[2]的值)
* 如果不选，那么则是`dp[3]`

因此状态转移方程可以是：`dp[i]= max(dp[i-1], dp[i-2]+nums[i-1])​`

### 代码
```go
func rob(nums []int) int {
	if len(nums) == 0 {
		return 0
	}
	dp := make([]int, len(nums)+1)
	dp[0] = 0
	dp[1] = nums[0]
	for i := 2; i <= len(nums); i++ {
		dp[i] = max(dp[i-1], dp[i-2]+nums[i-1])
	}
	return dp[len(nums)]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```


## 打家劫舍II
### 题目
​一个专业的小偷，计划偷窃一个环形街道上沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。
给定一个代表每个房屋存放金额的非负整数数组 nums ，请计算 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

```bash
示例 1：
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
示例 2：
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 3：
输入：nums = [0]
输出：0
```

### 分析
相比​打家劫舍​，这道题多了一个环形的限制。也就是说，在到达最后一个元素的时候，我们需要考虑 `i-2`是否是第一个元素，如果是第一个元素则不能选择.
那么此时我们就需要解决，当我已经选择了第一个元素，我最后一个元素就不能再去选择， 这么一个限制。
因为我们可以分为两种情况来分析, 也就是说，最后的结果是`max(nums[1:], nums[:len(nums)-1])​`

### 代码
```go
func rob(nums []int) int {
	if len(nums) == 0 {
		return 0
	}
	if len(nums) == 1 {
		return nums[0]
	}
	if len(nums) == 2 {
		return max(nums[0], nums[1])
	}

	dp1 := make([]int, len(nums)+1)
	dp2 := make([]int, len(nums)+1)
	dp1[0] = 0
	dp1[1] = nums[0]
	dp2[0] = 0
	dp2[1] = 0

	for i := 2; i <= len(nums); i++ {
		dp1[i] = max(dp1[i-1], dp1[i-2]+nums[i-1])
		dp2[i] = max(dp2[i-1], dp2[i-2]+nums[i-1])
	}

	return max(dp1[len(nums)-1], dp2[len(nums)])

}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```