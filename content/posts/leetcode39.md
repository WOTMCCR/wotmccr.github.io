---
categories: 算法题
title: 2024-06-28刷题2
tags:
  - leetcode
  - 动态规划
date: 2024-06-28
---
## 2024-06-28刷题2
### 121. 买卖股票的最佳时机
15:10
#### 题目描述
股票第i天的价格
只能选择在某一天买入这只股票，并选择在未来的某个不同的日子卖出
无法获利则返回0
#### 思路
贪心：
寻找左边最小，右边最大

#### 复杂度
时间复杂度：O(n)
空间复杂度：O(1)
#### 题解
##### C++
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int low = INT_MAX;
        int result = 0;
        for(int i = 0 ; i < prices.size() ; i++){
            low = min(low , prices[i]);
            result = max(prices[i] - low, result);
        }
        return result;
    }
};
```

### 122. 买卖股票的最佳时机 II
16:14
#### 思路
使用贪心算法吧，更简单，不能解决了，再动态规划
#### 题解
##### C++
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        //如果后一天大于前一天就直接出售
        for(int i = 1 ; i < prices.size() ; i++){
            if(prices[i] > prices[i-1]){
                result += prices[i] - prices[i-1];
            }
        }
        return result;
    }
};
```
### 123. 买卖股票的最佳时机 III
16:17
#### 题目描述
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。
#### 思路
##### dp数组下标以及含义
一天一共就有五个状态，
没有操作 （其实我们也可以不设置这个状态）
第一次**持有**股票
第一次不持有股票
第二次持有股票
第二次不持有股票
`dp[i][j]`中 i表示第i天，j为 `[0 - 4]` 五个状态
**`dp[i][j]`表示第i天状态j所剩最大现金。**

##### 确定递推公式
达到`dp[i][1]`状态，有两个具体操作：
- 操作一：第i天买入股票了，那么`dp[i][1] = dp[i-1][0] - prices[i]`
- 操作二：第i天没有操作，而是沿用前一天买入的状态，即：`dp[i][1] = dp[i - 1][1]`
一定是选最大的，所以 
`dp[i][1] = max(dp[i-1][0] - prices[i], dp[i - 1][1]);`

同理`dp[i][2]`也有两个操作：
- 操作一：第i天卖出股票了，那么`dp[i][2] = dp[i - 1][1] + prices[i]`
- 操作二：第i天没有操作，沿用前一天卖出股票的状态，即：`dp[i][2] = dp[i - 1][2]`
`dp[i][2] = max(dp[i - 1][1] + prices[i], dp[i - 1][2])`

同理可推出剩下状态部分：
`dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);`
`dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);`
##### 初始化
第0天没有操作，就是0，即：`dp[0][0] = 0`;
第0天做第一次买入的操作，`dp[0][1] = -prices[0]`;
第0天做第一次卖出的操作，`dp[0][2] = 0;`
第0天第二次买入操作，初始化为：`dp[0][3] = -prices[0]`;
第0天第二次卖出初始化`dp[0][4] = 0`;

##### 确定遍历顺序

从递归公式其实已经可以看出，一定是从前向后遍历，因为`dp[i]`，依靠`dp[i - 1]`的数值。
#### 复杂度
时间复杂度：O(n)
空间复杂度：O(n × 5)

#### 题解
##### C++
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0) return 0;
        //0~4 表示天的操作状态
        vector<vector<int>> dp (prices.size() , vector<int>(5,0));
        //初始化
        dp[0][1] = -prices[0];
        // dp[0][2] = 0; 
        dp[0][3] = -prices[0];
        // dp[0][4] = 0;
        //遍历
        for(int i = 1 ; i < prices.size() ; i++){
            //五个状态更新
            //0不操作
            dp[i][0] = dp[i-1][0];
            //1持有,前一天什么都没操作的情况下买入，与前天的持有状态
            dp[i][1] = max(dp[i-1][1],dp[i-1][0] - prices[i]);
            //卖出,保持前一天的卖出状态，前一天的买入状态卖出
            dp[i][2] = max(dp[i-1][2],dp[i-1][1] + prices[i]);
            //再持有
            dp[i][3] = max(dp[i-1][3],dp[i-1][2] - prices[i]);
            //再卖出
            dp[i][4] = max(dp[i-1][4],dp[i-1][3] + prices[i]);
        }
        return dp[prices.size()-1][4];
    }
};
```

