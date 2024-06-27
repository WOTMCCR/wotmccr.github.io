---
categories: 算法题
title: 2024-06-27刷题2
tags:
  - leetcode
  - 动态规划
date: 2024-06-27
---
## 2024-06-27刷题2
### 52. 携带研究材料（第七期模拟笔试）
17:18
#### 题目描述
行李箱所能承担的总重量为 N，携带最大价值的研究材料，每种研究材料可以选择无数次，并且可以重复选择。
#### 思路
完全背包问题
从小到大遍历
#### 题解
##### C++
```C++
#include<iostream>
#include<vector>
using namespace std;

void slove(vector<int> &weight,vector<int> &value,int N,int V){
    vector<int> dp(V+1,0);
    for(int i = 0 ; i < N ; i++){
        //从小到大遍历因为可以选择多次
        for(int j = weight[i] ; j <= V ; j++){
            dp[j] = max(dp[j] , dp[j - weight[i]] + value[i]);
        }
    }
    cout<<dp[V]<<endl;
}

int main(){
    int N,V;
    cin>>N>>V;
    vector<int> weight(N+1,0);
    vector<int> value(N+1,0);
    for(int i = 0 ; i < N ; i++){
        cin>>weight[i]>>value[i];
    }
    slove(weight,value,N,V);
}
```

### 518. 零钱兑换 II
17:33
#### 题目描述
coins不同面额的硬币，另一个整数amount表示总金额
返回凑成总金额的硬币组合数
#### 思路
**求装满背包有几种方法，公式都是：`dp[j] += dp[j - nums[i]]`;**
我们先来看 外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。

代码如下：

```C++
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

假设：`coins[0] = 1，coins[1] = 5`。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。

**所以这种遍历顺序中`dp[j]`里计算的是组合数！**

如果把两个for交换顺序，代码如下：

```C++
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。

**此时`dp[j]`里算出来的就是排列数！**
#### 复杂度
时间复杂度: O(mn)，其中 m 是amount，n 是 coins 的长度
空间复杂度: O(m)

#### 注意⚠️
dp首初始化为1，所有的都是增加该值
```C++
dp[0] = 1;
```
#### 题解
##### C++
```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount+1,0);
        dp[0] = 1;
        for(int i = 0 ; i < coins.size() ; i++){
            for(int j = coins[i] ; j <=amount ; j++){
                //求装满背包有几种方法,不加当前coin满足条件的，与加当前coin的
                dp[j] += dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
};
```

### 377. 组合总和 Ⅳ
19:47
#### 题目描述
从数组中找到，总和为target的元素组合个数
#### 思路
同完全背包问题，但是是排列思路
#### 复杂度
#### 注意⚠️
请注意，顺序不同的序列被视作不同的组合。

注意溢出
```C++
if (i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]) 
```
#### 题解
##### C++
```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target+1, 0);
        dp[0] = 1;
        for(int i = 0; i <= target ; i++){
            for(int j = 0 ; j < nums.size() ; j++){
                if (i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]) {
                    dp[i] += dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```



