---
categories: 算法题
title: 2024-06-29刷题2
tags:
  - leetcode
  - 动态规划
date: 2024-06-29
---
## 2024-06-29刷题2
### 1143. 最长公共子序列
15:47
#### 题目描述
两个字符串，返回两个字符串最长公共子序列的长度，可以不相连
#### 思路
仿照之前最长公共子数组与最大递增序列的思路
但是涉及到要处理两个数组的内容

确定dp数组的含义
`dp[i][j]`：长度为`[0, i - 1]`的字符串text1与长度为`[0, j - 1]`的字符串text2的最长公共子序列为`dp[i][j]`

确定递推公式
主要就是两大情况： 
`text1[i - 1]` 与 `text2[j - 1]`相同
`text1[i - 1]` 与 `text2[j - 1]`不相同

如果`text1[i - 1]` 与 `text2[j - 1]`相同，
那么找到了一个公共元素，所以`dp[i][j] = dp[i - 1][j - 1] + 1`;

如果`text1[i - 1]` 与 `text2[j - 1]`不相同，

那就看看`text1[0, i - 2]`与`text2[0, j - 1]`的最长公共子序列 
和 `text1[0, i - 1]`与`text2[0, j - 2]`的最长公共子序列，取最大的。

即：`dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])`;

#### 复杂度
时间复杂度: O(n * m)，其中 n 和 m 分别为 text1 和 text2 的长度
空间复杂度: O(n * m)
#### 注意⚠️
两层循环即是遍历过程，不用再需要从0开始比较
#### 题解
##### C++
```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {        
        vector<vector<int>> dp(text1.size() + 1 , vector<int>(text2.size()+1,0));
        for(int i = 1 ; i <= text1.size() ; i++){
            for(int j = 1 ; j <= text2.size() ; j++){
                if(text1[i-1] == text2[j-1]){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```


### 1035.不相交的线
17:30
#### 题目描述

#### 思路
绘制一些连接两个数字 `A[i]` 和 `B[j]` 的直线，只要 `A[i] == B[j]`，且直线不能相交！
直线不能相交，这就是说明在字符串A中 找到一个与字符串B相同的子序列，且这个子序列不能改变相对顺序，只要相对顺序不改变，链接相同数字的直线就不会相交。
所以就是求，最长公共子序列
#### 复杂度
时间复杂度: O(n * m)
空间复杂度: O(n * m)
#### 题解
##### C++
```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp (nums1.size() + 1 , vector<int>(nums2.size()+1 , 0));
        for(int i = 1 ; i <= nums1.size() ; i++){
            for(int j = 1 ; j <= nums2.size() ; j++){
                if(nums1[i-1] == nums2[j-1]){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = max(dp[i-1][j] , dp[i][j-1]);
                }
            }
        }
        return dp[nums1.size()][nums2.size()];
    }
};
```




### 53. 最大子序和
18:06
#### 思路
1. 确定dp数组（dp table）以及下标的含义
**`dp[i]`：包括下标i（以`nums[i]`为结尾）的最大连续子序列和为`dp[i]`**。

`dp[i]`只有两个方向可以推出来：
`dp[i - 1] + nums[i]`，即：`nums[i]`加入当前连续子序列和
`nums[i]`，即：从头开始计算当前连续子序列和
一定是取最大的，所以`dp[i] = max(dp[i - 1] + nums[i], nums[i])`;
#### 复杂度
时间复杂度：O(n)
空间复杂度：O(n)
#### 注意⚠️
`dp[i] = max(dp[i - 1] + nums[i], nums[i])`
当`nums[i]`的时候，及断开了
所以最后找到一个最大的dp值
#### 题解
##### C++
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        //i之前的最大连续子数组和
        vector<int> dp(nums.size()+1, 0);
        dp[0] = nums[0];
        int result = nums[0];
        for(int i = 1 ; i < nums.size(); i++){
            dp[i] = max(dp[i-1] + nums[i],nums[i]);
            if(dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```


### 392. 判断子序列
18:39
#### 题目描述
给定字符串s和t，判断s是否是t的子序列
#### 思路
贪心双指针
遍历t数组，如果相等则s数组索引加一，返回，s索引是否等于s的长度

动态规划
发现，所有两个序列相比较的题大多要有两个维度
dp含义
**`dp[i][j]` 表示以下标i-1为结尾的字符串s，和以下标j-1为结尾的字符串t，相同子序列的长度为`dp[i][j]`**。

确定递推公式
在确定递推公式的时候，首先要考虑如下两种操作，整理如下：
- `if (s[i - 1] == t[j - 1])`
	t中找到了一个字符在s中也出现了
- `if (s[i - 1] != t[j - 1])`
	相当于t要删除元素，继续匹配
`if (s[i - 1] == t[j - 1])`，那么`dp[i][j] = dp[i - 1][j - 1] + 1;`，因为找到了一个相同的字符，相同子序列长度自然要在`dp[i-1][j-1]`的基础上加1（如果不理解，在回看一下`dp[i][j]`的定义）

`if (s[i - 1] != t[j - 1])`，此时相当于t要删除元素，t如果把当前元素`t[j - 1]`删除，那么`dp[i][j]` 的数值就是 看`s[i - 1]`与 `t[j - 2]`的比较结果了，即：`dp[i][j] = dp[i][j - 1]`;


#### 复杂度
双指针
时间复杂度：O(n)

动态规划
时间复杂度：O(n × m)
空间复杂度：O(n × m)
#### 题解
##### C++
```C++
//双指针法
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int n = s.length() , m = t.length();
        int i = 0 , j = 0;
        while(i < n && j < m){
            if(s[i] == t[j]){
                i++;
            }
            j++;
        }
        return i == n;
    }
};

//动态规划
class Solution {
public:
    bool isSubsequence(string s, string t) {
        vector<vector<int>> dp(s.size()+1, vector<int> (t.size() + 1, 0));
        for(int i = 1 ; i <= s.size() ; i++){
            for(int j = 1 ; j <= t.size() ; j++){
                //如果相等
                if(s[i-1] == t[j-1]){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{ //不相等
                    //相当于t要删除元素，继续匹配
                    //t如果把当前元素t[j - 1]删除，那么dp[i][j] 的数值就是 看s[i - 1]与 t[j - 2]
                    dp[i][j] = dp[i][j-1];
                }
            }
        }
        return dp[s.size()][t.size()] == s.size();

    }
};
```
