---
title: "2025年12月14日 学习记录"
tags:
  - 日记
categories: 
  - life
date: 2025-12-14
draft: "false"
summary: 学习了一些leetcode的题目 主要是复习之前做过的 , 做的几道新题效果不佳需要第二天继续复习
---


## Excution Log

- Python 数组，预先创建指定长度的数组
	- `prefix = [1] * range`
- 两个数据类型的转换：
	- tuple做 key ：`s.add(tuple(sorted([i, j, k])))`
	- `tuple set -> list[list]` : `list(list(t) for t in set_t)`
- python 循环从前一层的下一个位置开始
	- `for i in range(n) :`
		- `for j in range(i+1,n):`
- nums.sort(reverse = True)
- 只根据指定元素排序
	- `intervals.sort(key=lambda x: x[0])`

## Thinking Log

做题和开发，一定要确定好数据结构，确定好每个数据到底是什么含义

1. **11. Container With Most Water**  
    - 移动长边相较于当前状态 必定减少面积
    - 移动短边 面积可能增加
2. **238. Product of Array Except Self**  
    - 确定好数据结构代表的意义，prefix当前位置之前元素的乘积
    - 确定好遍历的范围 1~range - 1 ，range - 1 ~ 0
    - 今天复习没有做出来，知道了前后缀相乘，但是，在更新上出了问题
3. **80. Remove Duplicates from Sorted Array II**  
    - 右指针遍历数组，左指针作为结果数组的边界，右指针的数据需要与当前 数组末尾元素-k 位置的元素进行比较
    - 思路知道但是没写出来，应该牢记 左指针代表什么 题中的数据结构代表什么
4. **26. Remove Duplicates from Sorted Array**  
    - 快慢指针，不断更新 right 指针，如果与 left 位置元素不一样则进行更新
5. **3. Longest Substring Without Repeating Characters** 
    - 滑动窗口 ，使用dict 存储 字符以及位置
        - 如果，right ch 在dict中，并且，index 处于窗口内
            - 更新left 为 dict[rightChar] + 1的位置
        - 将右边界元素添加道dict中
        - 计算长度
6. **128. Longest Consecutive Sequence**
    - 需要返回最长序列，这种题感觉需要先思考是从局部出发，还是需要整体判断
    - 可能是不连续的，所以可能散布再数组的各个地方，因此需要整体添加道set中进行判断
        - 查找头元素，如果 val - 1 并不处于set中说明当前元素为连续序列的起始

新题：

- **15. 3Sum**
	- 没什么思路，看题解了

- **209. Minimum Size Subarray Sum**  
	- 双指针大概知道思路，但是没写出来

- **42. Trapping Rain Water** 
	- 没写 打算 把三种方法都写一遍
- **56. Merge Intervals**  
	- 我的思路是按照左边界进行排序，然后遍历，思路正确但是没写出来
	- 具体细节考虑不完全
		- 如果答案数组为空 或者 答案数组末尾元素的值 与 新数组的起始值不相邻则直接添加
		- 否则将答案数组的末尾值设置为新数组的末尾值