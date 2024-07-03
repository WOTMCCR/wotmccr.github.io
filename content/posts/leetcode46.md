---
title: 2024-07-03刷题2
date: 2024-07-03
categories: 算法题
tags:
  - leetcode
  - 单调栈
---
## 2024-07-03 刷题2
### 42. 接雨水
22:02
#### 题目描述
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
#### 注意⚠️
求当前凹槽雨水的体积代码如下：
```C++
while (!st.empty() && height[i] > height[st.top()]) { // 注意这里是while，持续跟新栈顶元素
    int mid = st.top();
    st.pop();
    if (!st.empty()) {
        int h = min(height[st.top()], height[i]) - height[mid];
        int w = i - st.top() - 1; // 注意减一，只求中间宽度
        sum += h * w;
    }
}
```

#### 题解
##### C++
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> st;
        if(height.size() <= 2) return 0;
        st.push(0);
        int sum = 0;
        for(int i = 1 ; i < height.size() ; i++){
            if(height[i] < height[st.top()]){
                st.push(i);
            }else if(height[i] == height[st.top()]){
                //当相同高度的时候使用右侧的
                st.pop();
                st.push(i);
            }else{
                //计算体积
                while(!st.empty() && height[i] > height[st.top()]){
                    //左中右
                    int mid = st.top();
                    st.pop();
                    if(!st.empty()){
                        int h = min(height[st.top()] , height[i]) - height[mid];
                        int w = i - st.top() - 1;
                        sum += h * w;
                    }
                }
                st.push(i);
            }
        }
        return sum;
    }
};
```


### 84.柱状图中最大的矩形
22:28
#### 题目描述
给定 _n_ 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
#### 思路
单调栈，求相邻的相差最小的
#### 注意⚠️
计算体积的部分是一个while循环，每次都计算一次底乘高
比如单调栈中是1，2，4，5 这时候来了，3
则，while中的操作是
计算  $5 \times 1$
然后3仍然小于4 继续计算 $4 \times 2$
#### 题解
##### C++
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int result = 0;
        stack<int> st;
        heights.insert(heights.begin(), 0); // 数组头部加入元素0
        heights.push_back(0); // 数组尾部加入元素0
        st.push(0);

        for(int i = 1 ; i < heights.size() ; i++){
            if(heights[i] > heights[st.top()]){
                st.push(i);
            }else if(heights[i] == heights[st.top()]){
                st.pop();
                st.push(i);
            }else{
                while(!st.empty() && heights[i] < heights[st.top()]){
                    int mid = st.top();
                    st.pop();
                    if(!st.empty()){
                        int left = st.top();
                        int right = i;
                        int w = right - left -1;
                        int h = heights[mid] ;
                        result = max(result, w * h);
                    }
                }
                st.push(i);
            }
        }
        return result;
    }
};
```

