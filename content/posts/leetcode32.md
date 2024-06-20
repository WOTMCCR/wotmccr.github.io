---
categories: 算法题
title: " 2024-06-19题"
tags:
  - 动态规划
  - leetcode
date: 2024-06-19
---
## 2024-06-19 Wenseday
### 509. 斐波那契数
16:43
#### 思路
使用一个状态转移方程
```C++
        for(int i = 2; i <=n ; i++){
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
```
#### 题解
##### C++
```C++
class Solution {
public:
    int fib(int n) {
        if(n < 2) return n;
        int dp[2];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <=n ; i++){
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
        return dp[1];
    }
};
```
### 70. 爬楼梯
19:51
#### 思路
确定下表之后就要赶快，详细思考递推公式
#### 题解
##### C++
```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 1) return n;
        int dp[3];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3 ; i <= n ; i++){
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
};
```

### 746. 使用最小花费爬楼梯
20:27
#### 思路
到达第i台阶所花费的最少体力为`dp[i]`
递推公式
`dp[i] = min(dp[i-1] + cost[i-1],dp[i-2] + cost[i-2])` 
#### 复杂度
- 时间复杂度：O(n)
- 空间复杂度：O(1)
#### 注意⚠️
最开始可以从0开始，但是0并不在阶梯范围内
应该稍微忽视一点细节，动态规划应该更加注重状态
#### 题解
##### C++
```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        if(cost.size() < 2) return 0;
        int dp[2];
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2 ; i <=cost.size() ; i++){
            int feed = min(dp[1] + cost[i-1] , dp[0] + cost[i-2]);
            dp[0] = dp[1];
            dp[1] = feed;
        }
        return dp[1];
    }
};
```


