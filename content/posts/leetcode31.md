---
categories: 算法题
title: " 2024-06-18刷题2"
tags:
  - leetcode
  - 贪心
date: 2024-06-18
---
## 2024-06-18 刷题2
### 56. 合并区间
19:10
#### 思路
其实就是用合并区间后左边界和右边界，作为一个新的区间，加入到result数组里就可以了。
如果没有合并就把原区间加入到result数组。
#### 复杂度
时间复杂度: O(nlogn)
空间复杂度: O(logn)，排序需要的空间开销
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if(intervals.size() == 0) return result;
        sort(intervals.begin() , intervals.end() , [](const vector<int> &a , const vector<int> &b){return a[0] < b[0];});
        result.push_back(intervals[0]);        
        for(int i = 1 ; i < intervals.size() ; i++){
            if(result.back()[1] >= intervals[i][0]){
                result.back()[1] = max(result.back()[1] , intervals[i][1]);
                
            }else{
                result.push_back(intervals[i]);
            }
        }
        return result;
    }
};
```



### 738.单调递增的数字 
19:35
#### 思路
一旦出现`strNum[i - 1] > strNum[i]`的情况（非单调递增），首先想让`strNum[i - 1]--`，然后`strNum[i]`给为9，这样这个整数就是89，即小于98的最大的单调递增整数。

没想到的问题很大程度是对于语言的不熟悉，对语言的不熟悉限制了我的思路
#### 复杂度
时间复杂度：O(n)，n 为数字长度
空间复杂度：O(n)，需要一个字符串，转化为字符串操作更方便
#### 注意⚠️
```C++
string strNum = to_string(n);    
strNum[i-1]--;
return stoi(strNum);
```
#### 题解
##### C++
```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string strNum = to_string(n);    
        // 用来记录从什么时候开始记成9
        int flag = strNum.size();
        for(int i = strNum.size() - 1 ; i > 0 ; i--){
            if(strNum[i-1] > strNum[i]){
                flag = i;
                strNum[i-1]--;
            }
        }
        for(int i = flag ; i < strNum.size() ; i++){
            strNum[i] = '9';
        }
        return stoi(strNum);
    }
};
```

### 968. 监控二叉树
20:32
#### 思路
有如下三种：
- 该节点无覆盖
- 本节点有摄像头
- 本节点有覆盖
分别有三个数字来表示：
- 0：该节点无覆盖
- 1：本节点有摄像头
- 2：本节点有覆盖
#### 复杂度
- 时间复杂度: O(n)，需要遍历二叉树上的每个节点
- 空间复杂度: O(n)
#### 题解
##### C++
```C++
// 版本一
class Solution {
private:
    int result;
    int traversal(TreeNode* cur) {

        // 空节点，该节点有覆盖
        if (cur == NULL) return 2;

        int left = traversal(cur->left);    // 左
        int right = traversal(cur->right);  // 右

        // 情况1
        // 左右节点都有覆盖
        if (left == 2 && right == 2) return 0;

        // 情况2
        // left == 0 && right == 0 左右节点无覆盖
        // left == 1 && right == 0 左节点有摄像头，右节点无覆盖
        // left == 0 && right == 1 左节点有无覆盖，右节点摄像头
        // left == 0 && right == 2 左节点无覆盖，右节点覆盖
        // left == 2 && right == 0 左节点覆盖，右节点无覆盖
        if (left == 0 || right == 0) {
            result++;
            return 1;
        }

        // 情况3
        // left == 1 && right == 2 左节点有摄像头，右节点有覆盖
        // left == 2 && right == 1 左节点有覆盖，右节点有摄像头
        // left == 1 && right == 1 左右节点都有摄像头
        // 其他情况前段代码均已覆盖
        if (left == 1 || right == 1) return 2;

        // 以上代码我没有使用else，主要是为了把各个分支条件展现出来，这样代码有助于读者理解
        // 这个 return -1 逻辑不会走到这里。
        return -1;
    }

public:
    int minCameraCover(TreeNode* root) {
        result = 0;
        // 情况4
        if (traversal(root) == 0) { // root 无覆盖
            result++;
        }
        return result;
    }
};
```



