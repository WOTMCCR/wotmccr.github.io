---
date: 2024-05-06
tags:
  - leetcode
  - 字符串
title: 2024-05-16 刷题
description: 28. 实现 strStr()459.重复的子字符串，字符串总结 双指针回顾
---
## 2024-05-16
### 28.实现strStr()
14:40
#### 思路
需要计算出来next数组
- 从前到后遍历匹配字符串
	- 如果前后缀不同，则回退，没必要回退到开头，可以会退到上一个next对应的位置
	- 如果匹配则j++
然后更具next数组进行匹配
- 当不匹配的时候将j移动到`next[j]`的位置直到匹配成功
- 匹配成功则向后移动j
- j等于匹配串长度的时候则说明匹配成功
#### 复杂度
- 时间复杂度: O(n + m)
- 空间复杂度: O(m), 只需要保存字符串needle的前缀表
#### 题解
##### C++
```C++
class Solution {
public:

    //获得next表下一个
    void getNext(int* next, const string& s){
        //初始化
        int j = -1;
        next[0] = j;
        //从前往后遍历s字符串
        for(int i = 1 ; i < s.size() ; i++){
            //如果前后缀不同，则回退，没必要回退，到开头可以会退到上一个next对应的位置
            while(j >= 0 && s[i] != s[j+1]){
                j = next[j];
            }
            //如果找到相同的前后缀，那么就将前一个j的值加一
            if(s[i] == s[j+1]){
                j++;
            }
            next[i] = j;
        }
    }

    int strStr(string haystack, string needle) {
        if(needle.size() == 0){
            return 0;
        }
        vector<int> next(needle.size());
        getNext(&next[0],needle);
        int j = -1;
        //遍历haystack
        for(int i = 0 ; i < haystack.size() ; i++){
            //寻找nextj的位置
            while(j >= 0 && haystack[i] != needle[j+1]){
                j = next[j];
            }

            //匹配同时增加
            if(haystack[i] == needle[j+1]){
                j++;
            }
            
            //匹配成功
            if(j== (needle.size()-1)){
                return (i - needle.size() + 1);
            }
        }
        return -1;
    }
};
```

### 459.重复的子字符串
14:44
#### 思路
将两个s首尾连接在一起，里面还出现一个s的话，就说明是由重复子串组成。
为了防止，原本的s影响搜索结果，将首尾的，字符删除
#### 复杂度
- 时间复杂度: O(n)
- 空间复杂度: O(1)
#### 题解
##### C++
```C++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string t = s + s;
        t.erase(t.begin());
        t.erase(t.end()-1);
        if(t.find(s) != std::string::npos)
            return true;
        return false;
    }
};
```