---
title: 2025年12月17日 学习记录
created: 2025-12-17
tags:
  - 日记
categories:
  - life
draft: "false"
summary: 主要做了一些动态规划和贪心的题目
---
## Excution Log

- 从第二个元素开始 `for val in nums[1:] :` 
- any的操作 `dp[i] = any( 条件表达式 for x in wordDict if 过滤条件 )`
 
```python
 dp[i] = any(dp[i - len(w)] and s[i - len(w): i] == w 
             for w in wordDict 
             if len(w) <= i)
```
- max(里面必须有元素)，如果没有符合条件的 j 则，需要设置默认值，或拼接 `[1]`
	- `dp[i] = max([1] + [dp[j]+1 for j in range(i) if nums[j] < nums[i]] , default=1)`
## Thingking Log

### 复习题

**42. 接雨水**
- 必须写出三种解法：DP、双指针、单调栈
- dp一次过
- 单调栈的思路知道，但是具体实现的时候，入栈出栈的逻辑没写出来
- 双指针一遍过
**53. 最大子数组和**
- DP解法空间复杂度优化到 O(1)
-  `for val in nums[1:] :` 
**152. 乘积最大子数组**
- 空间复杂度 O(1)
- dp_min ,dp_max 代表上一个位置的最大 最小值
- 注意 temp 来存储 min
**55. 跳跃游戏**
- 一次遍历，O(n) 时间 O(1) 空间
- `cover = max(nums[i] + i , cover)`
**121. 买卖股票的最佳时机**
-  一次遍历，O(n) 时间 O(1) 空间
- 带着最小值 遍历价格数组
**713. 乘积小于K的子数组**
- 能清晰解释 `right - left + 1` 的含义
- 注意 单个元素 都大于 k 的情况 
	- `while prod >= k and left <= right: 可以让 right - left + 1 = 0 `
### 新题

**45. 跳跃游戏 II**
- 第一种方法类似于最长递增子序列的处理逻辑，因为保证能跳到最后，所以cover能到每一个位置
	- 计算 每一个 位置 之前的 的跳跃次数 如果 能跳的距离 + 位置 >= i，则进入比较
	- `dp[i] = min(dp[j] + 1 for j in range(i) if nums[j] + j >= i)`
**122. 买卖股票的最佳时机 II**
- 可以多次买卖
- 只要大于当前持仓的就卖出
- 如果小于当前持仓，更改持仓
**300. 最长递增子序列**
- `dp[i]`代表 以`nums[i]`结尾的 最长子序列
	- 每次需要遍历之前的所有内容，找到 小于当前元素的位置 并计算 `max(dp[j] + 1)`
	- `dp[i] = max([1] + [dp[j]+1 for j in range(i) if nums[j] < nums[i]])`
- 不是很有思路，但其实思路是对的，只是没有继续写下去
**139. 单词拆分** 
- `dp[i]` 代表 `dp[i] 表示：s 的前 i 个字符（s[:i]）能否被拼出来`
	- 递推公式为 `dp[i] = dp[i-len(w)] and s[i-len(w):i] == w`
- 大体的思路正确 ，但是被`dp[i]`的逻辑判断打扰了，并且少做了一个判断

### 明日
**300. 最长递增子序列**
- 时间复杂度 O(nlogn)
- 动态规划 + 二分优化
**45. 跳跃游戏 II**
- 贪心 + 动态规划解法
**122.买卖股票的最佳时机 II**
- 动态规划解法
**139.单词拆分** 
