---
layout:   page
title:       "动态规划 - 爬楼梯类型题目"
subtitle:    ""
description: ""
date:        2024-07-03
author:   "aiqiu"
#image:       "https://golearning.oss-cn-shanghai.aliyuncs.com/202304161638319.png"
categories:  ["Leetcode"]
published: true
---

# 动态规划 - 爬楼梯类型题目
## 什么是爬楼梯类型的题目
关于这种分类的形式其实是来自于 leetcode大神灵神分享的一个题单而来：https://leetcode.cn/circle/discuss/tXLS3i/
这类型的题目的解题样式都是： dp[i] = dp[i-1] + dp[i-2] .和爬楼梯的题目一样。换句话说，这类型的题目的都可以描述成爬楼梯的子问题。
### 爬楼梯问题
#### 原题
​假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

示例 1：
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶

#### 思考
这是一道经典的dp，思考到达楼顶后，可能是爬了一阶，或者是二阶。 问题就变成了求解 n-1, 和n-2时候有多少种可能的子问题，
变成dp函数的话就是：`dp[i] = dp[i-1] + dp[i-2]​`
这里的核心思维就是，我有两种选择，1和2，达到最终状态的时候肯定是前一个状态 + 两种选择决定的。也就是dp[i-1]​ 和dp[i-2]​ 的含义

#### 代码实现
```go
func climbStairs(n int) int {
    if n <=2 {
        return n
    }
    dp := make([]int, n+1)
    dp[1] = 1
    dp[2] = 2
    for i:=3;i<=n;i++{
        dp[i] = dp[i-1] + dp[i-2]
    }
    return dp[n]
}
```

### 组合总数IV
#### 原题
​给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。
题目数据保证答案符合 32 位整数范围。

示例 1：
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
示例 2：
输入：nums = [9], target = 3
输出：0

#### ​思考
这道题直觉上是使用回溯算法将所有的可能性列出来，然后符合条件的进行结果++，但是这样就超时了。
套用上面爬楼梯的四库，我们的题目可以变成，我们的楼梯高度是4，现在每次可以走 1，2，3层台阶，走到第四层的可能性有多少。 那么写成递推公式就是 dp[i]= dp[i-1] + dp[i-2]+d[i-3]

#### 代码实现

```go
func combinationSum4(nums []int, target int) int {
	if len(nums) == 0 {
		return 0
	}
    dp := make([]int, target+1)
    dp[0] = 1
    for i:=1;i<=target;i++{
        for _, num := range nums {
        // 需要确保不会 i-num 要大于等于0
            if i>=num{
                dp[i]+=dp[i-num]
            }
        }
    }
    return dp[target]
}
```