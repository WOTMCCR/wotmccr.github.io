---
categories: 算法
tags:
  - leetcode
  - 队列
  - 单调队列
  - 优先队列
description: " 239. 滑动窗口最大值 347.前 K 个高频元素 总结"
date: 2024-5-21
title: 2024-05-21刷题
---
## 2024-05-21
### 239.滑动窗口最大值
21:27
#### 思路
使用一个双端队列实现一个，单调队列，保证，队列中的元素，从大到小排列，
当使用push的时候，从后往前移除所有小于该值的元素
当使用pop(value)的时候，如果，pop的值等于队列中front的值，则pop。
使用front返回最大值，也就是第一个元素。

每次窗口移动的时候，
调用que.pop(滑动窗口中移除元素的数值)，移除窗口左边的元素，如果是最大值，则会pop成功，不是最大值，也不在队列里，不用处理
que.push(滑动窗口添加元素的数值)，将新元素按又大到小放到队列里
然后que.front()就返回我们要的最大值。
#### 复杂度
时间复杂度: O(n)
空间复杂度: O(k)
#### 题解
##### C++
```C++
class Solution {
//单调队列
private: 
    class Myqueue{
        public:p
            deque<int> que;
            //push进入队列,如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
            // 这样就保持了队列里的数值是单调从大到小的了。
            void push(int value){
                while(!que.empty() && value > que.back()){
                    que.pop_back();
                }
                que.push_back(value);
            }
            //pop(value)：如果窗口移除的元素value等于单调队列的出口元素，那么队列弹出元素，否则不用任何操作
            void pop(int value){
                while(!que.empty() && que.front() == value){
                    que.pop_front();
                }
            }

            //front返回队列中的最大值
            int front(){
                return que.front();
            }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        Myqueue que;
        vector<int> result;
        for(int i = 0 ; i < k ; i++){
            que.push(nums[i]);
        }
        result.push_back(que.front());
        for(int i = k ; i < nums.size() ; i++){
            que.pop(nums[i-k]);
            que.push(nums[i]);
            result.push_back(que.front());
        }
        return result;
    }
};

//multiset
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        multiset<int> st;
        vector<int> ans;
        for (int i = 0; i < nums.size(); i++) {
            if (i >= k) st.erase(st.find(nums[i - k]));
            st.insert(nums[i]);
            if (i >= k - 1) ans.push_back(*st.rbegin());
        }
        return ans;
    }
};
```

### 347.前k个高频元素
20:59
#### 思路
~~感觉和之前的那道题很像
可以有一个单调队列，然后，用来维持一个频率从高到低的数组
看了一下啊deque的用法，支持随机访问。
所以这个单调队列，不会pop值
front()返回最高的那个值
pop()
push()
~~
使用map来记录元素出现的次数，并且重载比较操作来构建优先队列。

使用优先队列来维护一个小顶堆来保存k个频率最大的元素。

之所以不使用大顶堆是因为，`priority_queue`中的`pop()`操作会移除队列顶部的元素，所以如果是使用的大顶堆当要保证优先队列中的元素个数为k个时，pop操作就会弹出最大值，而不是最小值。

用固定大小为k的小顶堆，扫面所有频率的数值，最后，输出优先队列中的元素与值。

#### 复杂度
时间复杂度: O(nlogk)
空间复杂度: O(n)
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    class Mycomparsion{
        public:
            bool operator()(const pair<int,int> &lhs, const pair<int,int> &rhs){
                return lhs.second > rhs.second;
            }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {

        unordered_map<int,int> map;
        for(int i = 0 ; i < nums.size() ; i++){
            map[nums[i]]++;
        }
        //用固定大小k的最小堆，扫描所有频率的数值
        priority_queue<pair<int,int>,vector<pair<int,int>>,Mycomparsion> pri_que;
        for(auto it = map.begin() ; it != map.end() ; it++){
            pri_que.push(*it);
            if(pri_que.size() > k){
                pri_que.pop();
            }
        }

        //输出，因为，是最小堆所以倒叙
        vector<int> result(k);
        for(int i = k - 1 ; i>=0 ; i--){
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;
    }
};
```


