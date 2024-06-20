---
categories: 算法题
title: " 2024-06-20题"
tags:
  - leetcode
  - 贪心
date: 2024-06-20
---
## 2024-06-20 Thursday
### 62. 不同路径
20:42
#### 思路
确定dp数组（dp table）以及下标的含义
`dp[i][j]` ：表示从（0 ，0）出发，到(i, j) 有`dp[i][j]`条不同的路径。
递推公式
`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`
dp数组的初始化
首先`dp[i][0]`一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么`dp[0][j]`也同理。

#### 复杂度
时间复杂度：O(m × n)
空间复杂度：O(m × n)
#### 注意⚠️
我觉的这个思路很简单我应该可以想到
但是，我之前，想的是使用一个大小为2的数组进行存储。因为这样是二维的可能有大于2的长度的多个元素，所以不能存储所有的信息。

所以做题应该不要先太在乎，空间复杂度，先做出来，再看能不能优化，不要限制自己。
#### 题解
##### C++
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i = 0 ; i < n ; i++) dp[0][i] = 1;
        for(int i = 0 ; i < m ; i++) dp[i][0] = 1;
        for(int i = 1 ; i < m ; i++){
            for(int j = 1 ; j < n ; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```
### 63. 不同路径 II
16:27
#### 思路
与第一道题一致，但是，初始化的时候以及赋值的时候需要，注意，是否有障碍物。

#### 复杂度
时间复杂度：O(n × m)，n、m 分别为obstacleGrid 长度和宽度
空间复杂度：O(n × m)
#### 注意⚠️
初始化的部分，当有一个障碍的时候，0行和0列，之后的就都为0了

#### 题解
##### C++
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
        for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
        for(int i = 1 ; i < m ; i++){
            for(int j = 1 ; j < n ; j++){
                if(obstacleGrid[i][j] == 1) dp[i][j]=0;
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

### 343. 整数拆分
17:14
#### 思路
`dp[i]`代表，本数最大的乘积
递推公式：
需要判断当前的数字大小
当前的大小
一个是`j * (i - j) `直接相乘。
一个是`j * dp[i - j]`，相当于是拆分(i - j)
` dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));`

#### 复杂度
时间复杂度：O(n^2)
空间复杂度：O(n)

#### 注意⚠️
遍历过程，需要遍历从1到当前数的所有可能，当前数/2

#### 题解
##### C++
```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1);
        dp[2] = 1;
        for(int i = 3 ; i <= n ; i++){
            for(int j = 1 ; j <= i/2 ; j++){
                //max(dp[i - j] * j , (i-j) * j)
                //一个是拆分一个是不拆
                //比如 2 ，不拆就大，拆就小
                dp[i] = max(dp[i] , max(dp[i - j] * j , (i-j) * j));
            }
        }
        return dp[n];
    }
};
```
### 96. 不同的二叉搜索树
18:09
#### 思路
递推公式意义：表示，当前节点数的二叉搜索树的种数
递推公式：
```C++
	for(int j = 1 ; j <= i ; j++){
	    dp[i] += dp[j-1] * dp[i-j]; //左右子树
    }
```
#### 复杂度
时间复杂度：$O(n^2)$
空间复杂度：$O(n)$
#### 题解
##### C++
```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1);
        dp[0] = 1;
        for(int i = 1 ; i <= n ; i++){
            for(int j = 1 ; j <= i ; j++){
                dp[i] += dp[j-1] * dp[i-j]; //左右子树
            }
        }
        return dp[n];
    }
};
```













