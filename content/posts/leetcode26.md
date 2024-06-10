---
categories: 算法题
tags:
  - leetcode
  - 回溯法
date: 2024-06-10
title: 2024-06-10刷题
---
## 2024-06-10
### 332. 重新安排行程
17:34
#### 思路
一个航线列表，感觉像是深度优先遍历，遍历到指定的目的地
但是可能有环
需要把所有ticket全部使用

有序：
这样存放映射关系可以定义为 `unordered_map<string, multiset<string>> targets` 或者 `unordered_map<string, map<string, int>> targets`。

含义如下：
```

unordered_map<string, multiset> targets：unordered_map<出发机场, 到达机场的集合> targets

unordered_map<string, map<string, int>> targets：unordered_map<出发机场, map<到达机场, 航班次数>> targets
```
这两个结构，我选择了后者，因为如果使用`unordered_map<string, multiset<string>> targets` 遍历multiset的时候，不能删除元素，一旦删除元素，迭代器就失效了。

```C++
// unordered_map<出发机场, map<到达机场, 航班次数>> targets
unordered_map<string, map<string, int>> targets;
```

只需要找到一个结果，所以回溯的返回值设置为bool

```C++
bool backtracking(int ticketNum, vector<string>& result) 
```

targets和result都需要初始化，代码如下：
```C++
for (const vector<string>& vec : tickets) {
    targets[vec[0]][vec[1]]++; // 记录映射关系
}
result.push_back("JFK"); // 起始机场
```

单层逻辑
循环取出可达集合中的元素
如果还有票则将其加入到结果集合中并，继续回溯
每次，从结果集合中的最后一个元素进行回溯
```C++
for (pair<const string, int>& target : targets[result[result.size() - 1]]) {
    if (target.second > 0 ) { // 记录到达机场是否飞过了
        result.push_back(target.first);
        target.second--;
        if (backtracking(ticketNum, result)) return true;
        result.pop_back();
        target.second++;
    }
}
```

#### 题解
##### C++
```C++
class Solution {
public:
    //出发机场，到达机场航班次数
    unordered_map<string,map<string,int>> targets;
    bool backtracking(int ticketNum , vector<string> &result){
        //比机票多一个机场
        if(result.size() == ticketNum + 1){
            return true;
        }

        //单层逻辑        
        for(pair<const string , int> & target : targets[result[result.size() - 1]]){
            //还有机票
            if(target.second > 0){
                result.push_back(target.first);
                target.second--;
                if(backtracking(ticketNum,result)) return true;
                result.pop_back();
                target.second++;
            }
        }
        return false;
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        vector<string> result;
        for(const vector<string> & vec : tickets){
            targets[vec[0]][vec[1]]++;            
        }
        result.push_back("JFK");
        backtracking(tickets.size() , result);
        return result;
    }
};
```

### 51. N 皇后
19:08
#### 思路
按照棋盘的行进行深度遍历
按照棋盘的格数进行横向遍历。
重点在与放置位置是否合法

深度是回溯的过程在做，不需要使用for来进行深度遍历
#### 题解
##### C++
```C++
class Solution {
public:
    vector<vector<string>> result;
    bool IsValid(int row , int col , int n ,vector<string> &path){
        //不能同列 不能同对角线
        for(int i = 0 ; i<row ; i++){
            if(path[i][col] == 'Q'){
                return false;
            }
        }
        //不能135度
        for(int i = row - 1 ,  j = col - 1 ; i >= 0 && j >= 0 ; i-- , j--){
            if(path[i][j] == 'Q'){
                return false;
            }
        }

        //不能45度
        for(int i = row - 1 ,  j = col + 1 ;  i >= 0 && j < n ; i-- , j++){
            if(path[i][j] == 'Q'){
                return false;
            }
        }
        
        return true;
    }
    void Backtracking(int n , int row , vector<string> &path){
        //当皇后的数量遍历结束，则结束
        if(row == n){
            result.push_back(path);
            return;
        }

        //单层逻辑
        for(int col = 0 ; col < n ; col++){
            //如果合法，复杂的判断逻辑可以抽象一下
            if(IsValid(row , col , n ,path)){
                path[row][col] = 'Q';
                Backtracking(n ,  row+1 ,path);
                path[row][col] = '.';
            }
        }
        return;
    }

    vector<vector<string>> solveNQueens(int n) {
        result.clear();
        vector<string> path(n , string(n,'.'));
        Backtracking(n,0,path);
        return result;
    }
};
```


### 37. 解数独
19:47
#### 思路
因为解数独找到一个符合的条件（就在树的叶子节点上）立刻就返回，相当于找从根节点到叶子节点一条唯一路径，所以需要使用bool返回值。
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
class Solution {
public:
    bool isValid(int row , int col, int val,vector<vector<char>> &board){
        for(int i = 0 ; i < 9 ;i++){
            if(board[row][i] == val){
                return false;
            }
        }
        for(int j = 0 ; j < 9 ;j++){
            if(board[j][col] == val){
                return false;
            }
        }
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
           for (int j = startCol; j < startCol + 3; j++) {
                if (board[i][j] == val ) {
                    return false;
                }
            }
        }
        return true;
    }
    //因为解数独找到一个符合的条件（就在树的叶子节点上）立刻就返回，相当于找从根节点到叶子节点一条唯一路径，所以需要使用bool返回值。
    bool backtracking(vector<vector<char>> &board){
        //遍历整个棋盘
        for(int i = 0 ; i < board.size() ; i++){
            for(int j = 0 ; j < board[0].size() ; j++){
                    if(board[i][j] == '.'){
                    //遍历1～9是否合法
                    for(char k = '1' ; k<= '9'; k++){
                        if(isValid(i,j,k,board)){
                            board[i][j] = k;
                            if(backtracking(board)) return true;
                            board[i][j] = '.';
                        }
                    }
                    //9个数都试完了就代表没有
                    return false;
                }
            }
        }
        return true;
    }

    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);        
    }
};
```









