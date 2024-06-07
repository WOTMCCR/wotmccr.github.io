---
categories: 算法题
title: 2024-06-07刷题
date: 2024-06-07
tags:
  - leetcode
  - 回溯法
description: ● 93.复原IP地址 ● 78.子集 ● 90.子集II
---
## 2024-06-07
### 93. 复原 IP 地址
20:42
#### 思路
也属于分割问题，只需要判断分割的数属于0～255即可
```C++
for (int i = startIndex; i < s.size(); i++) {
    if (isValid(s, startIndex, i)) { // 判断 [startIndex,i] 这个区间的子串是否合法
        s.insert(s.begin() + i + 1 , '.');  // 在i的后面插入一个逗点
        pointNum++;
        backtracking(s, i + 2, pointNum);   // 插入逗点之后下一个子串的起始位置为i+2
        pointNum--;                         // 回溯
        s.erase(s.begin() + i + 1);         // 回溯删掉逗点
    } else break; // 不合法，直接结束本层循环
}
```
#### 复杂度
- 时间复杂度: O(3^4)，IP地址最多包含4个数字，每个数字最多有3种可能的分割方式，则搜索树的最大深度为4，每个节点最多有3个子节点。
- 空间复杂度: O(n)
#### 题解
##### C++
```C++
class Solution {
private:
    vector<string> result;// 记录结果
    // startIndex: 搜索的起始位置，pointNum:添加逗点的数量
    void backtracking(string& s, int startIndex, int pointNum) {
        if (pointNum == 3) { // 逗点数量为3时，分隔结束
            // 判断第四段子字符串是否合法，如果合法就放进result中
            if (isValid(s, startIndex, s.size() - 1)) {
                result.push_back(s);
            }
            return;
        }
        for (int i = startIndex; i < s.size(); i++) {
            if (isValid(s, startIndex, i)) { // 判断 [startIndex,i] 这个区间的子串是否合法
                s.insert(s.begin() + i + 1 , '.');  // 在i的后面插入一个逗点
                pointNum++;
                backtracking(s, i + 2, pointNum);   // 插入逗点之后下一个子串的起始位置为i+2
                pointNum--;                         // 回溯
                s.erase(s.begin() + i + 1);         // 回溯删掉逗点
            } else break; // 不合法，直接结束本层循环
        }
    }
    // 判断字符串s在左闭又闭区间[start, end]所组成的数字是否合法
    bool isValid(const string& s, int start, int end) {
        if (start > end) {
            return false;
        }
        if (s[start] == '0' && start != end) { // 0开头的数字不合法
                return false;
        }
        int num = 0;
        for (int i = start; i <= end; i++) {
            if (s[i] > '9' || s[i] < '0') { // 遇到非数字字符不合法
                return false;
            }
            num = num * 10 + (s[i] - '0');
            if (num > 255) { // 如果大于255了不合法
                return false;
            }
        }
        return true;
    }
public:
    vector<string> restoreIpAddresses(string s) {
        result.clear();
        if (s.size() < 4 || s.size() > 12) return result; // 算是剪枝了
        backtracking(s, 0, 0);
        return result;
    }
};
```



### 78.子集
18:42
#### 思路
**组合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点！**

其实子集也是一种组合问题，因为它的集合是无序的，子集{1,2} 和 子集{2,1}是一样的。

**那么既然是无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始！**

**求取子集问题，不需要任何剪枝！因为子集就是要遍历整棵树**。
#### 复杂度
时间复杂度: O(n * 2^n)
空间复杂度: O(n)
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;    
    void Backtracking(vector<int>& nums,int startIndex){
        //每次都应该记录结果
        result.push_back(path);
        if(startIndex >= nums.size()){
            return;
        }

        for(int i = startIndex ; i < nums.size() ; i++){
            path.push_back(nums[i]);
            Backtracking(nums , i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear();
        path.clear();
        Backtracking(nums,0);
        return result;
    }
};
```

### 90.子集II
18:48
#### 思路
![[Screenshot 2024-06-07 at 19.03.56.png]]
#### 复杂度
#### 注意⚠️
**树层去重的话，需要对数组排序！**
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void Backtracking(vector<int>& nums , vector<bool> &traveled , int startIndex){
        result.push_back(path);
        //终止条件        
        if(startIndex >= nums.size()){
            return;
        }
        //循环条件
        for(int i = startIndex ; i < nums.size() ; i++){

            if(i>0 && traveled[i-1] == false && nums[i-1] == nums[i]){
                continue;
            }

            path.push_back(nums[i]);
            traveled[i] = true;
            Backtracking(nums,traveled,i+1);
            path.pop_back();
            traveled[i] = false;
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<bool> traveled(nums.size() , false);
        sort(nums.begin(),nums.end());
        Backtracking(nums,traveled,0);
        return result;
    }
};
```








