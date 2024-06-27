---
categories: 算法题
title: 2024-06-27刷题
tags:
  - leetcode
  - 动态规划
date: 2024-06-27
---
## 2024-06-27
### 1049. 最后一块石头的重量 II
12:51
用了一个小时
#### 题目描述
选出两块石头，然后相撞粉碎
重量相等则抵消，不等就消去小的，大的减去相同的重量
返回最后剩下的最小可能重量，没剩下就是返回0
 ```
示例 1：
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```
#### 思路
本题其实就是尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小，**这样就化解成01背包问题了**。
#### 复杂度
#### 注意⚠️
**在计算target的时候，target = sum / 2 因为是向下取整，所以`sum - dp[target]` 一定是大于等于`dp[target]`的**。
#### 题解
##### C++
```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        vector<int> dp(15001 , 0);
        int sum = 0;
        for(int i = 0 ; i < stones.size() ; i++) sum+=stones[i];
        int target = sum/2;
        for(int i = 0 ; i < stones.size() ; i++){
            for(int j = target ; j >= stones[i] ; j--){
                dp[j] = max(dp[j] , dp[j-stones[i]] + stones[i]);
            }
        }
        return sum - dp[target] - dp[target];
    }
};
```


### 494.目标和
14:37
#### 题目描述
非负整数数组nums 和整数target
往nums前添加运算符
返回通过上述方法构造的运算结果等于target的不同表达式的数目
#### 思路
可以使用回溯法遍历 $2^n$种可能，每个数都有+和-两种选项遍历，判断结果是否符合

动态规划的方法一定要先确定好公式的含义
然后确定递推公式

假设加法的总和为x，那么减法对应的总和就是sum - x。
所以我们要求的是 x - (sum - x) = target
x = (target + sum) / 2
此时问题就转化为，装满容量为x的背包，有几种方法。

`dp[j]` 表示：填满j（包括j）这么大容积的包，有`dp[j]`种方法
```C++
dp[j] += dp[j - nums[i]]
```




#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
//回溯法
//感觉可以当作蕾丝组合问题的模版
class Solution {
public:
    int count = 0;
    int findTargetSumWays(vector<int>& nums, int target) {
        backtrack(nums,target,0,0);
        return count;
    }
    void backtrack(vector<int> &nums,int target,int index,int sum){
        if(index == nums.size()){
            if(sum == target)
                count++;
        }else{
            backtrack(nums,target,index+1,sum+nums[index]);
            backtrack(nums,target,index+1,sum-nums[index]);
        }
    }
};

//动态规划
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        //求和
        int sum = 0;
        for(int i = 0 ; i < nums.size() ; i++) sum+=nums[i];
        //不符合条件的情况，总数少于target ，target+sum为奇数
        if(sum < abs(target) || (target+sum) % 2 == 1) return 0;
        //背包容量设置为加法和
        int bagSize = (target + sum) / 2;
        vector<int> dp(bagSize + 1, 0);
        //第一项设置为1否则，都为0
        dp[0] = 1;
        for(int i = 0 ; i < nums.size() ; i++){
            for(int j = bagSize; j >= nums[i] ; j--){
                //容量j的方法数
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
};
```

### 474. 一和零
16:02
#### 题目描述
二进制字符串数组集合 strS 和 两个整数m和n
找出并返回，strs的最大子集长度，该子集中最多有m个0和n个1
```
示例 1：
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```
#### 思路
1. dp数组下标以及含义
```
dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]。
```
2. 确定递推公式
```
dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。

dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。

dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
```

#### 复杂度
时间复杂度: O(kmn)，k 为strs的长度
空间复杂度: O(mn)
#### 题解
##### C++
```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1,vector<int>(n+1 , 0));
        for(string str : strs){
            int oneNum = 0;
            int ZeroNum = 0;
            for(char c: str){
                if(c == '0') ZeroNum++;
                else oneNum++;
            }
            //开始遍历
            for(int i = m ; i >= ZeroNum ; i--){
                for(int j = n ; j >= oneNum ; j--){
                    dp[i][j] = max(dp[i][j] , dp[i-ZeroNum][j-oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```




