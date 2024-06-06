---
categories: 算法题
title: 2024-06-06刷题
tags:
  - 回溯法
  - leetcode
date: 2024-06-06
description: 77题组合
---
## 2024-06-06
### 77. 组合
15:30
#### 思路
记录当前遍历到的位置，然后从当前位置的下一个继续进行遍历，遍历之后进行回溯
#### 复杂度
时间复杂度: O(n * 2^n)
空间复杂度: O(n)
#### 注意
可以当当前位置的元素
不满足要求的个数的时候就不继续遍历了
```C++
for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) { // 优化的地方
```
#### 题解
##### C++
```C++
class Solution {
public:
    //结果
    vector<vector<int>> result;
    //用于存放路径
    vector<int> path;
    void Backtracking(int cur,int n,int k){
        if(path.size() == k){
            result.push_back(path);
            return;
        }
        for(int i = cur ; i <= n - (k - path.size()) + 1 ; i++){
            path.push_back(i);
            Backtracking(i+1,n,k);
            path.pop_back();
        }        
    }
    vector<vector<int>> combine(int n, int k) {
        Backtracking(1,n,k);
        return result;
    }
};
```



