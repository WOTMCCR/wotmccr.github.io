---
tags:
  - leetcode
  - 栈
  - 队列
date: 2024-05-17
title: 2024-05-17刷题
description: 使用栈以及队列模拟队列和栈
---
## 2024-05-17
### 使用栈模拟队列
16:49
#### 思路
使用两个栈，stkIN，stkOut
当入队列的时候，先入In栈
当出队列的时候，将in栈的所有元素都移到out栈中，然后，out栈pop
#### 题解
##### C++
```C++
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;
    MyQueue() {

    }
    
    void push(int x) {
        stIn.push(x);
    }
    
    int pop() {
        if(stOut.empty()){
            while(!stIn.empty()){
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }
    
    int peek() {
        int res = this->pop();
        stOut.push(res);
        return res;
    }
    
    bool empty() {
        return stIn.empty() && stOut.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */v
```
###  使用队列模拟栈
16:51
#### 思路
使用一个队列，当出栈的时候，将除最后一个元素外的所有元素出队列并重新入到队列末尾
然后pop最后一个元素
#### 题解
##### C++
```C++
class MyStack {
public:
    queue<int> que;
    MyStack() {

    }
    
    void push(int x) {
        que.push(x);
    }
    
    int pop() {
        int size = que.size();
        size--;
        while(size--){
            que.push(que.front());
            que.pop();
        }
        int result = que.front();
        que.pop();
        return result;
    }
    
    int top() {
        return que.back();
    }
    
    bool empty() {
        return que.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```









