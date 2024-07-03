---
title: 2024-07-03刷题
tags:
  - leetcode
  - 单调栈
date: 2024-07-03
categories: 算法题
---
## 2024-07-03 刷题
### 739. 每日温度
15:39
#### 题目描述
给一个整数数组，表示每天的温度，
返回一个数组，`answer[i]`表示对于第i天，下一个更高温度出现在几天后，
如果之后气温都不会升高则在该位置用0来代替
#### 思路
构造一个，从栈顶到栈底递增的单调栈。
然后一个循环，遍历所有元素，当元素大于栈顶的元素的时候就将栈中的元素弹出，直到新元素小于栈中元素或者栈中元素为空。

#### 复杂度
时间复杂度：O(n)
空间复杂度：O(n)
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        vector<int> result(temperatures.size() , 0);
        st.push(0);
        for(int i = 1 ; i < temperatures.size() ; i++){
            while(!st.empty()){
                if(temperatures[i] > temperatures[st.top()]){
                    result[st.top()] = i - st.top();
                    st.pop();
                }else if(temperatures[i] < temperatures[st.top()]){
                    break;
                }else{
                    break;
                }
            }
            st.push(i);
        }
        return result;
    }
};
```

### 496.下一个更大元素 I
21:22
#### 思路
维持一个单调栈，栈中的元素是nums1中的元素的时候将对应情况的答案写到result中
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result (nums1.size(),-1);
        if(nums1.size() <= 1) return result;
        stack<int> st;
        unordered_map<int,int> umap;
        for(int i  = 0 ; i < nums1.size() ; i++){
            umap[nums1[i]] = i;
        }
        st.push(0);
        //遍历nums1找j
        for(int i = 0 ; i < nums2.size() ; i++){
            if(nums2[i] < nums2[st.top()]){
                st.push(i);
            }else if(nums2[i] == nums2[st.top()]){
                st.push(i);
            }else{
                while(!st.empty() && nums2[i] > nums2[st.top()]){
                    if(umap.count(nums2[st.top()]) > 0){
                        int index = umap[nums2[st.top()]]; 
                        result[index] = nums2[i];
                    }
                    st.pop();
                }
                st.push(i);
            }
        }
        return result;
    }
};
```

### 503. 下一个更大元素 II
21:49
#### 题目描述
循环数组，寻找下一个更大的元素，不存在则返回-1
#### 思路
将两个nums数组拼接在一起，使用单调栈计算出每一个元素的下一个最大值，最后再把结果集即result数组resize到原数组大小就可以了。

#### 注意⚠️
循环遍历数组
```C++
 st.push(i%nums.size());
```
#### 题解
##### C++
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> result(nums.size() , -1);
        stack<int> st;
        st.push(0);
        for(int i = 0 ; i < nums.size() * 2; i++){
            if(nums[i % nums.size()] < nums[st.top()]){
                st.push(i%nums.size());
            }else if(nums[i % nums.size()] == nums[st.top()]){
                st.push(i%nums.size());
            }else{
                while(!st.empty() && nums[i%nums.size()] > nums[st.top()]){
                    result[st.top()] = nums[i%nums.size()];
                    st.pop();
                }
                st.push(i%nums.size());
            }
        }
        return result;
    }
};
```
