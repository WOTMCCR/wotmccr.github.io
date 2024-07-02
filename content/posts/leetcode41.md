---
categories: 算法题
title: 2024-06-29刷题
tags:
  - leetcode
  - 动态规划
date: 2024-06-29
---
## 2024-06-29刷题
### 300. 最长递增子序列
13:12
#### 题目描述
不改变数组中元素顺序的最长子序列
#### 思路
遍历当前位置之前的所有子序列长度，如果，当前元素大于该元素则更新递推公式
```C++
	for(int j = 0 ; j < i ; j++){
        if(nums[i] > nums[j]) 
	        dp[i] = max(dp[i],dp[j] + 1);
    }
```
#### 复杂度
时间复杂度: O(n^2)
空间复杂度: O(n)
#### 题解
##### C++
```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.size() <= 1) return nums.size();
        vector<int> dp(nums.size() , 1);
        int result = 0;
        for(int i = 1 ; i < nums.size() ; i++){
            for(int j = 0 ; j < i ; j++){
                if(nums[i] > nums[j]) dp[i] = max(dp[i],dp[j] + 1);
            }
            if(dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

### 674. 最长连续递增序列
14:32
#### 题目描述
连续的最长递增序列
#### 思路
贪心：
每次选取最长的子序列

动态规划
只需要一层循环即可
#### 复杂度
动态规划
时间复杂度：O(n)
空间复杂度：O(n)

贪心
时间复杂度：O(n)
空间复杂度：O(1)
#### 注意⚠️
#### 题解
##### C++
```C++
//贪心
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int result = 0;
        int temp = 0;
        for(int i = 1 ; i < nums.size() ; i++){
            if(nums[i] > nums[i-1]) {
                temp++;
                if(temp>result)
                    result = temp;
            }
            else{
                temp = 0;
            }
        }
        return result + 1;
    }
};

//动态规划
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size() <= 1) return nums.size();
        vector<int> dp(nums.size() , 1);
        int result = 0 ;
        for(int i = 1 ; i < nums.size() ; i++){
            if(nums[i] > nums[i-1])
                dp[i] = dp[i-1] + 1;
            if(dp[i] > result)
                result = dp[i];
        }
        return result;
    }
};
```

### 718. 最长重复子数组
15:12
#### 题目描述
两个数组num1 和 num2 返回，两个数组中公共的，长度最长的子数组的长度
#### 思路
dp含义
`dp[i][j]` ：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为`dp[i][j]`

递推公式
根据`dp[i][j]`的定义，`dp[i][j]`的状态只能由`dp[i - 1][j - 1]`推导出来。
即当`A[i - 1]`和`B[j - 1]`相等的时候，`dp[i][j] = dp[i - 1][j - 1] + 1`;
根据递推公式可以看出，遍历i 和 j 要从1开始！
```C++
    for(int j = 1 ; j <= nums2.size() ; j++){
        if(nums1[i-1] == nums2[j-1])
            dp[i][j] = dp[i-1][j-1] + 1;
        if(dp[i][j] > result)
            result = dp[i][j];
    }
```
#### 复杂度
时间复杂度：O(n × m)，n 为A长度，m为B长度
空间复杂度：O(n × m)
#### 题解
##### C++
```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1 , vector<int>(nums2.size() + 1 , 0));
        int result = 0;
        for(int i = 1 ; i <= nums1.size() ; i++){
            for(int j = 1 ; j <= nums2.size() ; j++){
                if(nums1[i-1] == nums2[j-1])
                    dp[i][j] = dp[i-1][j-1] + 1;
                if(dp[i][j] > result)
                    result = dp[i][j];
            }
        }
        return result;
    }
};
```

