---
categories: 算法题
title: " 2024-06-18刷题1"
tags:
  - leetcode
  - 贪心
date: 2024-06-18
---
## 2024-06-18 刷题1
重叠区间一类
距离上次刷题过去了，三天
我不记得周六日做了什么了，大致是，周六下午考了六级，周日拍了毕业照，也没有干什么事情。
如果相较于之前，我想是，人很难在一个动态的时间里保持高效。
### 452. 用最少数量的箭引爆气球
15:23
#### 思路
每次尽可能引爆更多的气球
感觉贪心一般都需要排序一下，会好很多
感觉很巧妙
进行遍历，关注右侧重叠的部分
如果，上一个气球与本气球相交，则说明，只需要一个气球，
但是，为了保证下个气球仍能覆盖前一个气球，当前气球的右边界与之前气球的右边界进行比较选取最小值，并更新当前气球的右边界。
如果，当前气球不与之前的气球相交了，则所需要的弓箭+1。
#### 复杂度
- 时间复杂度：O(nlog n)，因为有一个快排
- 空间复杂度：O(1)，有一个快排，最差情况(倒序)时，需要n次递归调用。
#### 注意⚠️
`points[i][0]` $x_{start}$
`points[i][1]` $x_{end}$
#### 题解
##### C++
```C++
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {        
        if(points.size() == 0) return 0;
        sort(points.begin() , points.end() , cmp);
        int num = 1;
        for(int i = 1 ; i < points.size() ; i++){
            if(points[i][0] > points[i-1][1]){
                num++;
            }else{
                points[i][1] = min(points[i-1][1],points[i][1]);
            }
        }
        return num;
    }
};
```
### 435. 无重叠区间
16:15
#### 思路
排序
然后遍历判断是否重叠，如果重叠则+1

这好像是另一种想法，只要在一个交叉区间内就必定会相交
所以只能留下交叉区间个线段
减去交叉区间的个数就是，需要移除的个数

因为是按照右端点的位置排序，所以，当新的左端点，大于右端点的时候则区间加一
```C++
    if(end <= intervals[i][0]){
        end = intervals[i][1];
        count++;
    }
```

#### 复杂度
时间复杂度：O(nlog n) ，有一个快排
空间复杂度：O(n)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    static bool cmp(const vector<int> a , const vector<int> b){
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if(intervals.size() ==1) return 0;
        sort(intervals.begin() ,intervals.end() ,cmp);
        int count = 1; // 记录非交叉区间的个数
        int end = intervals[0][1]; // 记录区间分割点
        for(int i = 0 ; i < intervals.size() ; i++){
            if(end <= intervals[i][0]){
                end = intervals[i][1];
                count++;
            }
        }
        return intervals.size() - count;

    }
};

class Solution {
public:
    static bool cmp (const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0]; // 改为左边界排序
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int count = 0; // 注意这里从0开始，因为是记录重叠区间
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < intervals[i - 1][1]) { //重叠情况
                intervals[i][1] = min(intervals[i - 1][1], intervals[i][1]);
                count++;
            }
        }
        return count;
    }
};

```

### 763. 划分字母区间
17:29
#### 思路
需要先遍历一遍寻找当前字母出现的最远距离
```C++
    for(int i = 0 ; i < s.size() ; i++){
        hash[s[i] - 'a'] = i;
    }
```
然后，将最近距离与最远距离作为一个区间
之后，应用之前几道题的逻辑
从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点
![[Screenshot 2024-06-18 at 17.34.27.png]]
#### 复杂度
时间复杂度：O(n)
空间复杂度：O(1)，使用的hash数组是固定大小
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[27] = {0};
        for(int i = 0 ; i < s.size() ; i++){
            hash[s[i] - 'a'] = i;
        }
        vector<int> result;
        int left = 0;
        int right = 0;
        for(int i = 0 ; i < s.size() ; i++){
            right = max(right,hash[s[i] - 'a']);
            if(i == right){
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
    }
};
```


