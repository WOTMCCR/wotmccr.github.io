---
categories: 算法题
title: 2024-07-02刷题2
tags:
  - leetcode
  - 动态规划
date: 2024-07-02
---
## 2024-07-02 刷题2
### 647. 回文子串
18:46
#### 题目描述
给你一个字符串 s ，请你统计并返回这个字符串中 回文子串 的数目。
#### 思路
递推公式含义
布尔类型的`dp[i][j]`：表示区间范围`[i,j]` （注意是左闭右闭）的子串是否是回文子串，如果是`dp[i][j]`为true，否则为false。

递推公式
当`s[i]和s[j]`不相等的时候 dp为false
当相等的时候，如果i和j相等则为一个字母，dp为true
i和j相差一个，则例如aa，也是回文子串
相差大于1，则dp为`dp[i+1][j-1]`的值
#### 题解
##### C++
```C++
class Solution {
public:
    int countSubstrings(string s) {
        //布尔类型的dp[i][j]：表示区间范围[i,j] （注意是左闭右闭）的子串是否是回文子串，如果是dp[i][j]为true，否则为false。
        vector<vector<bool>> dp(s.size() , vector<bool> (s.size(), false));
        int result = 0;
        for(int i = s.size()-1 ; i >= 0 ; i--){
            for(int j = i ; j < s.size() ; j++){
                if(s[i] == s[j]){
                    if(j - i <= 1){
                        result++;
                        dp[i][j] = true;
                    }else if(dp[i+1][j-1]){
                        result++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        return result;
    }
};
```

### 516.最长回文子序列
19:25
#### 题目描述
给一个数组，找出其中最长的回文子序列，并返回该序列的长度
#### 思路
确定dp数组（dp table）以及下标的含义
`dp[i][j]`：字符串s在`[i, j]`范围内最长的回文子序列的长度为`dp[i][j]`。


#### 复杂度
#### 注意⚠️
**初始化**
所以需要手动初始化一下，当i与j相同，那么`dp[i][j]`一定是等于1的，即：一个字符的回文子序列长度就是1。

其他情况`dp[i][j]`初始为0就行，这样递推公式：`dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]); `中`dp[i][j]`才不会被初始值覆盖。

```C++
max(dp[i + 1][j], dp[i][j - 1]);
```

遍历
从递归公式中，可以看出，`dp[i][j]` 依赖于 `dp[i + 1][j - 1]` ，`dp[i + 1][j]` 和 `dp[i][j - 1]`
**所以遍历i的时候一定要从下到上遍历，这样才能保证下一行的数据是经过计算的。**
#### 题解
##### C++
```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size() , vector<int>(s.size() , 0));
        for(int i = 0 ; i<s.size() ; i++) dp[i][i] = 1;

        for(int i = s.size() - 1 ; i >= 0 ; i--){
            for(int j = i + 1 ; j < s.size() ; j++){
                if(s[i] == s[j]){
                    dp[i][j] = dp[i+1][j-1] + 2;
                }else{
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size()-1];
    }
};
```
