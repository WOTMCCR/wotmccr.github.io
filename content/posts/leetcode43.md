---
categories: 算法题
title: 2024-07-02刷题
tags:
  - leetcode
  - 动态规划
date: 2024-07-02
---
## 2024-07-02 刷题
### 115.不同的子序列
14:59
#### 题目描述
给定两个字符串s和t，统计并返回，s的子序列中t出现的个数
#### 思路
`dp[i][j]`：以i-1为结尾的s子序列中出现以j-1为结尾的t的个数为`dp[i][j]`。
删除不匹配的元素的思路，
`dp[i][j] = dp[i-1][j];` 当前匹配位置的值，等于等于i-1位置的值
#### 复杂度
时间复杂度: O(n * m)
空间复杂度: O(n * m)
#### 题解
##### C++
```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint64_t>> dp(s.size() + 1 , vector<uint64_t>(t.size()+1 , 0));
        for(int i = 0 ; i < s.size() ; i++) dp[i][0] = 1;
        for(int j = 1 ; j < t.size() ; j++) dp[0][j] = 0;
        for(int i = 1 ; i <= s.size() ; i++){
            for(int j = 1 ; j <= t.size() ; j++){
                if(s[i-1] == t[j-1]){
                    //一部分是用s[i - 1]来匹配，那么个数为dp[i - 1][j - 1]。即不需要考虑当前s子串和t子串的最后一位字母，所以只需要 dp[i-1][j-1]。
                    //一部分是不用s[i - 1]来匹配，个数为dp[i - 1][j]。
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                }else{
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[s.size()][t.size()];
    }
};
```

### 583. 两个字符串的删除操作
15:30
#### 题目描述
给定两个单词 word1 和 word2 ，返回使得 word1 和  word2 相同所需的最小步数。
每步 可以删除任意一个字符串中的一个字符。
#### 思路
纯粹的删除
递推公式含义`dp[i][j]`为i-1 j-1位置单词相等的最小步数
#### 复杂度
时间复杂度: O(n * m)
空间复杂度: O(n * m)
#### 注意⚠️
初始化的部分
从递推公式中，可以看出来，`dp[i][0] 和 dp[0][j]`是一定要初始化的。
`dp[i][0]`：word2为空字符串，以i-1为结尾的字符串word1要删除多少个元素，才能和word2相同呢，很明显`dp[i][0] = i`。
```C++
vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1));
for (int i = 0; i <= word1.size(); i++) dp[i][0] = i;
for (int j = 0; j <= word2.size(); j++) dp[0][j] = j;
```
#### 题解
##### C++
```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1 , vector<int>(word2.size() + 1 , 0));
        for (int i = 0; i <= word1.size(); i++) dp[i][0] = i;
        for (int j = 0; j <= word2.size(); j++) dp[0][j] = j;
        for(int i = 1 ; i <= word1.size() ; i++){
            for(int j = 1 ; j<= word2.size() ; j++){
                if(word1[i-1] != word2[j-1]){
                    dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + 1;
                }else{
                    dp[i][j] = dp[i-1][j-1];
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
};

//动态规划
//只要求出两个字符串的最长公共子序列长度即可，那么除了最长公共子序列之外的字符都是必须删除的，最后用两个字符串的总长度减去两个最长公共子序列的长度就是删除的最少步数。
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size()+1, vector<int>(word2.size()+1, 0));
        for (int i=1; i<=word1.size(); i++){
            for (int j=1; j<=word2.size(); j++){
                if (word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return word1.size()+word2.size()-dp[word1.size()][word2.size()]*2;
    }
};
```

### 72. 编辑距离
17:10
#### 题目描述
两个单词，返回最小操作数，对一个单词可以进行如下三种操作：
- 插入一个字符
- 删除一个字符
- 替换一个字符
#### 思路
##### 确定dp数组
`dp[i][j]` 表示以下标i-1为结尾的字符串word1，和以下标j-1为结尾的字符串word2，最近编辑距离为`dp[i][j]`
##### 递推公式
```C++
if (word1[i - 1] == word2[j - 1])
    不操作
if (word1[i - 1] != word2[j - 1])
    增
    删
    换
```

增删元素：
`if (word1[i - 1] != word2[j - 1])`
操作一：word1删除一个元素，那么就是以下标i - 2为结尾的word1 与 j-1为结尾的word2的最近编辑距离 再加上一个操作。
即` dp[i][j] = dp[i - 1][j] + 1`;
操作二：word2删除一个元素，那么就是以下标i - 1为结尾的word1 与 j-2为结尾的word2的最近编辑距离 再加上一个操作。
即 `dp[i][j] = dp[i][j - 1] + 1`;
word2添加一个元素，相当于word1删除一个元素，
例如 word1 = "ad" ，word2 = "a"，
word1删除元素'd' 和 word2添加一个元素'd'，
变成word1="a", word2="ad"， 最终的操作数是一样

替换元素:
word1替换`word1[i - 1]`，使其与`word2[j - 1]`相同，此时不用增删加元素。
那么只需要一次替换的操作，就可以让 `word1[i - 1] 和 word2[j - 1]` 相同。
所以` dp[i][j] = dp[i - 1][j - 1] + 1`;

##### 初始化
`dp[i][0]` ：以下标i-1为结尾的字符串word1，和空字符串word2，最近编辑距离为`dp[i][0]`。
那么`dp[i][0]`就应该是i，对word1里的元素全部做删除操作，即：`dp[i][0] = i`;
```C++
for (int i = 0; i <= word1.size(); i++) dp[i][0] = i;
for (int j = 0; j <= word2.size(); j++) dp[0][j] = j;
```
#### 注意⚠️
删除word2相当于增加word1
#### 题解
##### C++
```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size()+1 , vector<int>(word2.size()+1,0));
        for(int i = 0 ; i <= word1.size() ; i++) dp[i][0] = i;
        for(int j = 0 ; j <= word2.size() ; j++) dp[0][j] = j;
        for(int i = 1 ; i <= word1.size() ; i++){
            for(int j = 1 ; j <= word2.size() ; j++){
                if(word1[i-1] == word2[j-1]){
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    //删除word2相当于增加word1
                    dp[i][j] = min({dp[i-1][j-1],dp[i-1][j],dp[i][j-1]}) + 1;
                }

            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```




