---
categories: 算法题
title: " 2024-06-11刷题"
tags:
  - leetcode
  - 贪心
date: 2024-06-11
---
## 2024-06-11
### 122. 买卖股票的最佳时机 II
09:59
#### 思路
感觉，每一次买股票如果当前价格高于持有的价格就卖出
但是第一次的股票价格如果很高，那就会一直持有，

那感觉还是最大子序列和类似，只不过，不需要在意之前的部分

只需要当前元素大于之前的元素就加上差值即可，因为，连续上升的股票，也可以每天都买卖

也就是
**终利润是可以分解的，那么本题就很容易了！**

假如第 0 天买入，第 3 天卖出，那么利润为：`prices[3] - prices[0]`。

相当于`(prices[3] - prices[2]) + (prices[2] - prices[1]) + (prices[1] - prices[0])`。

**此时就是把利润分解为每天为单位的维度，而不是从 0 天到第 3 天整体去考虑！**

那么根据 prices 可以得到每天的利润序列：`(prices[i] - prices[i - 1]).....(prices[1] - prices[0])`。
#### 题解
##### C++
贪心
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
### 55. 跳跃游戏
10:14
#### 思路
必定从0下标开始
因为，能够选择最大长度内的任意点，所以感觉，本质是求能够到达的最远距离。

记录一个最远的距离，然后，遍历距离内的元素，看能否再增加


感觉，可以从数学角度直接求出结果，因为已知数组长度

#### 注意⚠️
```C++
for(int i = 0 ; i<=cover ;i++)
```
#### 题解
##### C++
```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        if(nums.size() == 1) return true;
        for(int i = 0 ; i<=cover ;i++){
            cover = max(i+nums[i] , cover);
            if(cover >= nums.size()-1) return true;
        }
        return false;
    }
};
```



### 45. 跳跃游戏 II
11:32
#### 思路
最小跳跃次数
就是移动下标达到了当前覆盖的最远距离下标时，步数就要加一，来增加覆盖距离

使用一个变量记录，记录最远的距离
```C++
nextDistance = max(nextDistance , i + nums[i]);
```
#### 题解
##### C++
```C++
class Solution {
public:
    int jump(vector<int>& nums) {
            if(nums.size() == 1) return 0;
            int curDistance = 0;
            int ans = 0;
            int nextDistance = 0;
            for(int i = 0 ; i < nums.size() ; i++){
                nextDistance = max(nextDistance , i + nums[i]);
                if(i == curDistance){
                    ans++;
                    curDistance = nextDistance;
                    if(nextDistance >= nums.size() - 1)
                        break;
                }
            }
            return ans;
        }
};
```








- [ ] 