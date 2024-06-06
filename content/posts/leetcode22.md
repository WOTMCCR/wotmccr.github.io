---
categories: 算法题
tags:
  - leetcode
  - 回溯法
date: 2024-06-06
title: 2024-06-06刷题2
description: 216.组合总和III  17. 电话号码的字母组合
---
## 2024-06-06刷题2
### 216.组合总和III
15:44
#### 思路
与找到所有数的组合的遍历思路一致
在终止条件的部分进行验证判断数据的和是否满足条件，如果满足条件才将路径添加到结果集合中
并且在处理和回溯的部分，对与sum值进行处理。
#### 剪枝
```C++
if (sum > targetSum) { // 剪枝操作
    return;
}
```
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    int sum = 0;
    void backtracing(int cur, int n , int k){
        if(path.size() == k){
            if(sum == n)
            {
                result.push_back(path);
            }            
            return;
        }

        for(int i = cur ; i <= 9 - (k - path.size()) + 1 ; i++ ){
            path.push_back(i);
            sum+=i;
            backtracing(i+1,n,k);
            sum-=i;
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        backtracing(1,n,k);
        return result;
    }
};
```
### 17. 电话号码的字母组合
16:06
#### 思路
看这个，每个数字对应不同的字母应该要先根据按的按键确定数据集
```C++
const string letterMap[10] = {
    "", // 0
    "", // 1
    "abc", // 2
    "def", // 3
    "ghi", // 4
    "jkl", // 5
    "mno", // 6
    "pqrs", // 7
    "tuv", // 8
    "wxyz", // 9
};
```

参数，参数指定是有题目中给的string digits，然后还要有一个参数就是int型的index。

注意这个index可不是 [77.组合 (opens new window)](https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html)和[216.组合总和III (opens new window)](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html)中的startIndex了。

这个index是记录遍历第几个数字了，就是用来遍历digits的（题目中给出数字字符串），同时index也表示树的深度。

```C++
vector<string> result;
string s;
void backtracking(const string& digits, int index)
```

#### 复杂度
时间复杂度: O(3^m * 4^n)，其中 m 是对应四个字母的数字个数，n 是对应三个字母的数字个数

空间复杂度: O(3^m * 4^n)

#### 注意⚠️
这个index是记录遍历第几个数字了，就是用来遍历digits的（题目中给出数字字符串），同时index也表示树的深度。

**常量引用 `const string& s`**：传递字符串的引用以避免不必要的拷贝。
尽管它是一个引用，但在调用递归函数时，你将 `s` 与 `letters[i]` 相加形成一个新的字符串
#### 题解
##### C++
```C++
class Solution {
private:
    const string letterMap[10] = {
            "", // 0
            "", // 1
            "abc", // 2
            "def", // 3
            "ghi", // 4
            "jkl", // 5
            "mno", // 6
            "pqrs", // 7
            "tuv", // 8
            "wxyz", // 9
        };
public:
    vector<string> result;
    void Backtracing(const string &digits,int index , const string& s){
        if(index == digits.size()){
            result.push_back(s);
            return;
        }
        int digit = digits[index] - '0';
        string letter = letterMap[digit];
        for(int i = 0; i < letter.size() ; i++){
            Backtracing(digits,index+1,s+letter[i]);
        }
    }
    vector<string> letterCombinations(string digits) {
        result.clear();
        if(digits.size() == 0){
            return result;
        }
        Backtracing(digits,0,"");
        return result;
    }
};
```








