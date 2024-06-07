---
categories: 算法题
tags:
  - leetcode
  - 回溯法
date: 2024-06-07
description: "* 491.递增子序列* 46.全排列* 47.全排列 II"
title: 2024-06-07刷题2
---

## 2024-06-07刷题2
### 491. 非递减子序列
19:16
#### 思路
![[../static/attachments/Screenshot 2024-06-07 at 19.37.00.png]]
**同一父节点下的同层上使用过的元素就不能再使用了**
#### 复杂度
#### 注意⚠️
**`unordered_set<int> uset;` 是记录本层元素是否重复使用，新的一层uset都会重新定义（清空），所以要知道uset只负责本层！**
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void Backtracking(vector<int> &nums , int startIndex){
        if(path.size() > 1){
            result.push_back(path);            
        }

        //每一层的去重
        unordered_set<int> uset;
        for(int i = startIndex ; i<nums.size();i++){
            if((!path.empty() && nums[i] < path.back())
                ||(uset.find(nums[i]) != uset.end())){
                    continue;
                }
            uset.insert(nums[i]);
            path.push_back(nums[i]);
            Backtracking(nums,i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        Backtracking(nums,0);
        return result;
    }
};
```

### 46. 全排列
19:49
#### 思路
**使用used数组，记录此时path里都有哪些元素使用了，一个排列里一个元素只能使用一次**。
每次都从头到位进行元素的选取
#### 复杂度
- 时间复杂度: O(n!)
- 空间复杂度: O(n)
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int> &nums , vector<bool> &used){
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        for(int i = 0 ; i < nums.size() ; i++){
            //如果，已经有了则跳过
            if (used[i] == true) continue;            

            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums,used);
            path.pop_back();
            used[i] = false;
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size() , false);
        backtracking(nums,used);
        return result;
    }
};
```

### 47.全排列 II
https://programmercarl.com/0047.全排列II.html#拓展
20:15
#### 思路
**去重一定要对元素进行排序，这样我们才方便通过相邻的节点来判断是否重复使用了**。
应该是一个行去重，并且标记当前使用了哪个元素，来进行排列。
```
used[i - 1] == true，说明同一树枝nums[i - 1]使用
used[i - 1] == false，说明同一树层nums[i - 1]使用过
```
#### 复杂度
#### 注意⚠️
**对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高！**
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void Backtracking(vector<int> &nums,vector<bool> &used){
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        

        for(int i = 0 ; i < nums.size() ; i++ ){
            if(used[i] == true || (i>0 && nums[i] == nums[i-1] && used[i-1] == false)) continue;
            // if(i>0 && nums[i] == nums[i-1]) continue;
            
            path.push_back(nums[i]);
            used[i] = true;
            Backtracking(nums,used);
            used[i] = false;
            path.pop_back();
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> used(nums.size() , false);
        sort(nums.begin() , nums.end());
        Backtracking(nums,used);
        return result;
    }
};
```









