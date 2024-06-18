---
categories: 算法题
title: " 2024-06-15刷题"
tags:
  - 贪心
  - leetcode
date: 2024-06-15
---
## 2024-06-15 刷题
距离上次周二已经过去了4天
大致这四天完成了，游戏开发的大作业，以及补充完善了一点多媒体系统导论的内容。

今天看看能够做多少题？
### 134. 加油站
18:17
#### 思路
首先如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站 剩油量`rest[i]`相加一定是大于等于零的。
对每个加油站，到达下一个加油站的剩余油量进行求和
如果区间`[0,i]`的和为负数，则说明，从0跑不到i的位置
所以从i+1的位置再开始

#### 复杂度
- 时间复杂度：O(n)
- 空间复杂度：O(1)
#### 题解
##### C++
```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
                start = i + 1;  // 起始位置更新为i+1
                curSum = 0;     // curSum从0开始
            }
        }
        if (totalSum < 0) return -1; // 说明怎么走都不可能跑一圈了
        return start;
    }
};
```


### 135. 分发糖果
20:13
#### 思路
两次遍历
从左往右进行遍历
如果右边的元素大于左边的元素则，左侧元素+1
从右往左进行遍历
如果左侧的元素大于右侧的元素，则，选取右+1与当前位置最大的元素
#### 复杂度
- 时间复杂度: O(n)
- 空间复杂度: O(n)
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int sum = 0;
        vector<int> candy (ratings.size() , 1);
        for(int i = 1; i < ratings.size(); i++){
            if(ratings[i] > ratings[i-1]){
                candy[i] = candy[i-1] + 1;
            }
        }

        for(int i = ratings.size() - 2 ; i >= 0 ; i--){
            if(ratings[i] > ratings[i+1]){
                candy[i] = max(candy[i] , candy[i+1] + 1);
            }
        }
        for(int i = 0 ; i < candy.size() ; i++){
            sum+=candy[i];
        }
        return sum;
    }
};
```
### 860. 柠檬水找零
20:22
#### 思路
自己的零钱
需要判断来的是5 10 20 中的哪一种，因为找零很固定
#### 复杂度
时间复杂度: O(n)
空间复杂度: O(1)
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0, twenty = 0;
        for (int bill : bills) {
            // 情况一
            if (bill == 5) five++;
            // 情况二
            if (bill == 10) {
                if (five <= 0) return false;
                ten++;
                five--;
            }
            // 情况三
            if (bill == 20) {
                // 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
                if (five > 0 && ten > 0) {
                    five--;
                    ten--;
                    twenty++; // 其实这行代码可以删了，因为记录20已经没有意义了，不会用20来找零
                } else if (five >= 3) {
                    five -= 3;
                    twenty++; // 同理，这行代码也可以删了
                } else return false;
            }
        }
        return true;
    }
};
```
### 406.根据身高重建队列
11:36
#### 思路
这道题完全没有任何的思路
看题解是，先按照身高排序，然后从高到低根据k进行插入
但是为什么能想到这样呢？
题目要求是最后排列的结果是，第二个数据的值为，大于他元素的个数。
#### 题解
##### C++
```C++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin() , people.end() , cmp);
        vector<vector<int>> que;
        for(int i = 0 ; i < people.size() ; i++){
            int position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
};
```









