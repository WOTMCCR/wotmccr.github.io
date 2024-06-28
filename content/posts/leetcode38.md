---
categories: 算法题
tags:
  - leetcode
  - 动态规划
title: 2024-06-28刷题1
date: 2024-06-28
---
## 2024-06-28刷题1
### 198. 打家劫舍
11:29
#### 题目描述
给定存放金额的数组
相邻的房屋装有互通的防盗系统，如果相邻房屋被盗会触发警报
计算在不触发警报的情况下，能够偷到的最高金额
#### 思路
又是选择物品，装进背包类的问题
但是，好像所有动态规划的问题都有类似的感觉，所以这应该是能使用动态规划的思路

确定dp为当前房屋的最高金额
递推公式
偷前一个房屋，不偷当前房屋
与偷当前房屋的最大值
`dp[j] = max(dp[j - 2] + num[j] , dp[j-1])`


#### 复杂度
O(n)
#### 注意⚠️
初始化的部分，第二个位置的值应该为前两个前两个中的最大值
需要牢牢记住dp为偷到当前房屋已经偷到的最高金额
```C++
	dp[0] = nums[0];
    dp[1] = max(nums[0] , nums[1]);
```
#### 题解
##### C++
```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() < 2) return nums[0];
        vector<int> dp(nums.size() + 1, 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0] , nums[1]);
        for(int i = 2 ; i < nums.size() ; i++){
            dp[i] = max(dp[i - 2] + nums[i] , dp[i-1]);
        }
        return dp[nums.size()-1];
    }
};
```

### 213. 打家劫舍 II
12:25
#### 题目描述
我现在是一个专业的小偷，房屋围成了一个圈
同样相邻的房屋被偷会触发警报，所以，在，触发警报的情况下，计算能够偷到的最高金额。
#### 思路
对于一个数组，成环的话主要有如下三种情况：
对于一个数组，成环的话主要有如下三种情况：

- 情况一：考虑不包含首尾元素

![213.打家劫舍II](https://code-thinking-1253855093.file.myqcloud.com/pics/20210129160748643-20230310134000692.jpg)

- 情况二：考虑包含首元素，不包含尾元素

![213.打家劫舍II1](https://code-thinking-1253855093.file.myqcloud.com/pics/20210129160821374-20230310134003961.jpg)

- 情况三：考虑包含尾元素，不包含首元素

![213.打家劫舍II2](https://code-thinking-1253855093.file.myqcloud.com/pics/20210129160842491-20230310134008133.jpg)

**而情况二 和 情况三 都包含了情况一了，所以只考虑情况二和情况三就可以了**。
#### 复杂度
时间复杂度: O(n)
空间复杂度: O(n)
#### 注意⚠️
边界条件
以及，处理环的方法，不计算最后一个元素，与不计算第一个元素，两种情况。
```C++
    int result1 = robRange(nums,0,nums.size()-2);
    int result2 = robRange(nums,1,nums.size()-1);
```

#### 题解
##### C++
```C++
class Solution {
public:
    int robRange(vector<int> &nums , int start , int end){
        if (end == start) return nums[start];
        vector<int> dp(nums.size(),0);
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start], nums[start + 1]);
        for(int i = start + 2 ; i <= end; i++){
            dp[i] = max(dp[i-2]+nums[i] , dp[i-1]);
        }
        return dp[end];
    }
    int rob(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        if(nums.size()<2) return nums[0];
        //成环循环有两种情况        
        int result1 = robRange(nums,0,nums.size()-2);
        int result2 = robRange(nums,1,nums.size()-1);
        return max(result1,result2);
    }

};
```

### 337. 打家劫舍 III
14:26
#### 题目描述
小偷发现一个新的可窃地区，只有一个入口，类似一棵二叉树，报警规则相同。
给定root，在不触动警报的情况下，小偷能盗取的最高金额。
#### 思路
 偷与不偷的两个状态所得到的金钱，那么返回值就是一个长度为2的数组。
 dp数组（dp table）以及下标的含义：
 - 下标为0记录不偷该节点所得到的的最大金钱，
-  下标为1记录偷该节点所得到的的最大金钱。
 
首先明确的是使用后序遍历。 因为要通过递归函数的返回值来做下一步计算。
通过递归左节点，得到左节点偷与不偷的金钱。
通过递归右节点，得到右节点偷与不偷的金钱。

如果是偷当前节点，那么左右孩子就不能偷，`val1 = cur->val + left[0] + right[0];` （**如果对下标含义不理解就再回顾一下dp数组的含义**）

如果不偷当前节点，那么左右孩子就可以偷，至于到底偷不偷一定是选一个最大的，所以：`val2 = max(left[0], left[1]) + max(right[0], right[1]);`

最后当前节点的状态就是{val2, val1}; 即：{不偷当前节点得到的最大金钱，偷当前节点得到的最大金钱}
#### 复杂度
时间复杂度：O(n)，每个节点只遍历了一次
空间复杂度：O(log n)，算上递推系统栈的空间
#### 注意⚠️
因为左左右子树中间会相隔节点，所以不能使用迭代法先获得数组之后再套用之前的解法
#### 题解
##### C++
```C++
class Solution {
public:
    int rob(TreeNode* root) {
        vector<int> result = robTree(root);
        return max(result[0] , result[1]);
    }
    vector<int> robTree(TreeNode *cur){
        if(cur == nullptr) return vector<int>{0,0};
        // 长度为2的数组，0：不偷，1：偷
        vector<int> left  = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        //偷当前节点，偷了之后就不能偷左右节点
        int val1 = cur->val + left[0] + right[0];
        //不偷当前节点，可以再偷或者不偷左右节点
        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2,val1};
    }
};

```
