---
categories: 算法题
title: 2024-06-27刷题3
tags:
  - leetcode
  - 动态规划
date: 2024-06-27
---
## 2024-06-27刷题3
### 322. 零钱兑换
20:24
#### 题目描述
coins数组，计算凑成总金额所需的最少硬币个数
#### 思路
```C++
dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
```
#### 复杂度
时间复杂度: O(n * amount)，其中 n 为 coins 的长度
空间复杂度: O(amount)
#### 题解
##### C++
```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for(int i = 0 ; i < coins.size() ; i++){
            for(int j = coins[i] ; j <= amount ; j++){
                if (dp[j - coins[i]] != INT_MAX) { // 如果dp[j - coins[i]]是初始值则跳过
                    dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
                }
            }
        }
        if(dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```



### 279. 完全平方数
21:24
#### 题目描述
给定整数n，返回和为n的完全平方数的最少数量
#### 思路
将平方数作为物品
平方数可以使用`i*i`来表示
#### 题解
##### C++
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;
        for(int i = 1 ; i * i <= n ; i++){ //物品
            for(int j = i*i ; j <= n ; j++){ //背包
                dp[j] = min(dp[j] , dp[j-i*i] + 1);
            }
        }
        return dp[n];
    }
};

```

### 139.单词拆分
22:33
#### 题目描述
使用字符串列表作为字典，能否拼接出字符串s
#### 思路
选取字符串列表组合的过程。
可以重复使用所以，完全背包问题

所有的递推公式都是根据题目的答案来确定的。
**`dp[i]` : 字符串长度为i的话，`dp[i]`为true，表示可以拆分为一个或多个在字典中出现的单词**。
#### 注意⚠️
求的是排列数
本题一定是 先遍历 背包，再遍历物品。
#### 题解
##### C++
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for(int i = 1; i <= s.size(); i++) { //背包
            for(int j = 0; j < wordDict.size(); j++) { //物品
                if(i >= wordDict[j].size()) {
                    string tmp = s.substr(i - wordDict[j].size(), wordDict[j].size());
                    if(tmp == wordDict[j] && dp[i - wordDict[j].size()]) 
                        dp[i] =true;
                }
            }
        }
        return dp[s.size()];
    }
};
```
