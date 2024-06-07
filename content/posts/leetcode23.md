---
categories: 算法题
tags:
  - leetcode
  - 回溯法
title: 2024-06-06刷题3
date: 2024-06-06
description: 切割问题
---
## 2024-06-06刷题3
### 39.组合总和
16:35
#### 思路
主要是可以重复选取，并且不限制数量，这两点是与之前不太一样的部分

应该不用刻意考虑去重，因为，是一个无重复元素的整数数组
#### 复杂度
时间复杂度: O(n * 2^n)，注意这只是复杂度的上界，因为剪枝的存在，真实的时间复杂度远小于此
空间复杂度: O(target)
#### 注意⚠️
我一直再想如何循环，trace元素相同的情况，但其实
直接如下即可
```C++
        for(int i = index ; i < candidates.size() ; i++){
            result.push_back(candidates[i]);
            sum+=candidates[i];
            Backtracking(candidates,target,i);                
            result.pop_back(candidates[i]);
            sum-=candidates[i];
        }
```
![[../static/attachments/Screenshot 2024-06-06 at 17.00.08.png]]
相当于在下一层中，再选取下一个元素。
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> results;
    vector<int>result;
    int sum;
    void Backtracking(vector<int>& candidates , int target , int index){
        // if(sum > target){
        //     return;
        // }
        if(sum == target){
            results.push_back(result);
            return;
        }
        //这里剪枝需要对与数列进行排序，否则，可能有后面的元素仍然满足情况
        for(int i = index ; i < candidates.size() && sum +candidates[i] <= target; i++){
            result.push_back(candidates[i]);
            sum+=candidates[i];
            Backtracking(candidates,target,i);
            result.pop_back();
            sum-=candidates[i];
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        Backtracking(candidates,target,0);
        return results;
    }
};
```





### 40.组合总和II
19:17
#### 思路
**集合（数组candidates）有重复元素，但结果还不能有重复的组合**。

**所以我们要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重**。
![[Screenshot 2024-06-06 at 19.26.12.png]]

需要使用一个数组，来存储元素是否使用`vector<bool>& used `
![[Screenshot 2024-06-06 at 19.30.25.png]]
可以看出在candidates[i] == candidates[i - 1]相同的情况下：
- used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
- used[i - 1] == false，说明同一树层candidates[i - 1]使用过
#### 复杂度
#### 注意⚠️
```C++
backtracking(candidates, target, sum, i + 1, used);
```
和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次

**强调一下，树层去重的话，需要对数组排序！**
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> results;
    vector<int> result;
    int sum = 0;    
    void Backtracking(vector<int> &candidates,int target , int index ,vector<bool>& used)
    {
        if(sum == target){
            results.push_back(result);
            return;
        }
        for(int i = index ; i < candidates.size() && sum+candidates[i] <= target ; i++){
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if(i>0 && candidates[i] == candidates[i-1] && used[i-1] == false)
            {
                continue;
            }
            result.push_back(candidates[i]);
            sum+=candidates[i];
            used[i] = true;

            Backtracking(candidates,target,i+1,used);

            used[i] = false;
            sum -= candidates[i];
            result.pop_back();            
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);

        sort(candidates.begin(),candidates.end());
        Backtracking(candidates,target,0 ,used);
        return results;
    }
};
```

### 131. 分割回文串
20:08
#### 思路
这是一个切割问题,也可以抽象为一棵树形结构
在`for (int i = startIndex; i < s.size(); i++)`循环中，我们 定义了起始位置startIndex，那么 `[startIndex, i]` 就是要截取的子串。
#### 复杂度
#### 注意⚠️
**注意切割过的位置，不能重复切割，所以，backtracking(s, i + 1); 传入下一层的起始位置为i + 1**。
#### 题解
##### C++
```C++
class Solution {
public:
    bool isPalindrome(const string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
    vector<vector<string>> results;
    vector<string> path;
    void backtracking(const string &s , int index){
        if(index >= s.size()){
            results.push_back(path);
            return;
        }
        for(int i = index ; i < s.size() ; i++){
            if (isPalindrome(s, index, i)) { // 是回文子串
               // 获取[startIndex,i]在s中的子串
                string str = s.substr(index, i - index + 1);
                path.push_back(str);
            } else {                // 如果不是则直接跳过
                continue;
            }
            backtracking(s,i+1);// 寻找i+1为起始位置的子串
            path.pop_back();
        }
    }
    vector<vector<string>> partition(string s) {
        backtracking(s,0);
        return results;
    }
};
```





