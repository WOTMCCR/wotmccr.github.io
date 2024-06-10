---
categories: 算法题
title: " 2024-06-10刷题2"
tags:
  - leetcode
  - 贪心
date: 2024-06-10
---
## 2024-06-10
### 455. 分发饼干
20:45
#### 思路
感觉需要排序，从大到小排序
最大的饼干给胃口最大的孩子，如果满足不了，则不可能满足

#### 复杂度
时间复杂度：O(nlogn)
空间复杂度：O(1)

#### 题解
##### C++
```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        //使用贪心
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        //从大到小
        int index = s.size() - 1;
        int result = 0;
        //孩子从大到小
        for(int i = g.size() - 1 ; i >= 0 ; i--){
            //饼干
            if(index >= 0 && s[index] >= g[i]){
                result++;
                index--;
            }
        }
        return result;
    }
};
```


### 376. 摆动序列
21:00
#### 思路
**这就是贪心所贪的地方，让峰值尽可能的保持峰值，然后删除单一坡度上的节点**
#### 题解
##### C++
```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size() <= 1) return nums.size();
        int curdiff = 0;
        int prediff = 0;
        int result = 1;
        for(int i = 0 ; i < nums.size()-1 ; i++){
            curdiff = nums[i+1] - nums[i];
             // 出现峰值
            if ((prediff <= 0 && curdiff > 0) || (prediff >= 0 && curdiff < 0)) {
                result++;
                prediff = curdiff; // 注意这里，只在摆动变化的时候更新prediff
            }
        }
        return result;
    }
};
```

### 53. 最大子数组和
21:28
#### 思路
如果添加的下一个数，导致结果小于0，则将count设置为0，重新开始。
#### 题解
##### C++
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT_MIN;
        int count = 0;
        for(int i = 0 ; i<nums.size() ; i++){
            count+=nums[i];
            //当count小于0的时候就可以重新开始了
            if(count > result){
                result = count;
            }
            if(count <= 0) count=0;
        }
        return result;
    }
};
```







