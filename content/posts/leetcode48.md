---
title: 2024-07-04刷题2
tags:
  - leetcode
  - 图论
date: 2024-07-04
categories: 算法题
---
## 2024-07-04 刷题2
### 99. 岛屿数量(深搜版)
09:36
#### 题目描述
给定一个由1陆地和0水组成的矩阵，计算岛屿数量
#### 思路
是用遇到一个没有遍历过的节点陆地，计数器就加一，然后把该节点陆地所能遍历到的陆地都标记上。
在遇到标记过的陆地节点和海洋节点的时候直接跳过。 这样计数器就是最终岛屿的数量。
#### 复杂度
#### 注意⚠️
```C++
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
```
#### 题解
##### C++
```C++
#include<iostream>
#include<vector>
using namespace std;

int counter = 0;

int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向

void dfs(const vector<vector<int>> & graph , vector<vector<bool>> &visited ,int x ,int y){
    //遍历4个方向
    for(int i = 0 ; i < 4 ; i++){
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if (nextx < 0 || nextx >= graph.size() || nexty < 0 || nexty >= graph[0].size()) continue; 
        if(graph[nextx][nexty] == 1 && !visited[nextx][nexty]){
            visited[nextx][nexty] = true;
            dfs(graph,visited,nextx,nexty);
        }
    }
}

int main(){
    int m,n;
    cin>>n>>m;
    int value = 0;
    vector<vector<int>> graph (n,vector<int>(m,0));
    for(int i = 0 ; i < n ; i++){
        for(int j = 0 ; j < m ; j++){
            cin>>value;
            graph[i][j] = value;
        }
    }
    vector<vector<bool>> visited(n, vector<bool>(m, false));
    int result = 0;
    for(int i = 0 ; i < n; i++){
        for(int j = 0 ; j < m; j++){
            if(!visited[i][j] && graph[i][j] == 1){
                visited[i][j]= true;
                result++;
                dfs(graph,visited,i,j);
            }
        }
    }
    cout<<result<<endl;
    
}
```

### 99. 岛屿数量 (广搜版)
10:14
#### 题目描述

#### 思路
维护一个队列，使用bfs进行遍历
#### 复杂度
#### 注意⚠️
**只要 加入队列就代表 走过，就需要标记，而不是从队列拿出来的时候再去标记走过**。

注意使用引用修改visited的值
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void bfs(const vector<vector<int>> &grid,vector<vector<bool>> &visited,int x,int y){
    queue<pair<int,int>> que;
    que.push({x,y});
    visited[x][y] = true;
    while(!que.empty()){
        pair<int,int> cur = que.front();
        que.pop();
        int curx = cur.first;
        int cury = cur.second;
        for(int i = 0 ; i < 4 ; i++){
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
                que.push({nextx, nexty});
                visited[nextx][nexty] = true;
          }
        }
    }
}

int main(){
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    vector<vector<bool>> visited(n, vector<bool>(m, false));

    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == 1) {
                result++; // 遇到没访问过的陆地，+1
                bfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
            }
        }
    }
    cout<<result;
}
```

### 100. 岛屿的最大面积
10:41
#### 思路
遍历岛屿的时候更新岛屿大小

#### 题解
##### C++
```C++
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
int count;
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void dfs(const vector<vector<int>> &graph , vector<vector<bool>> &visited,int x ,int y){
    for(int i = 0 ; i < 4 ; i++){
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if(nextx < 0 || nexty < 0 || nextx >= graph.size() || nexty >= graph[0].size()) continue;
        if(!visited[nextx][nexty] && graph[nextx][nexty] == 1){
            visited[nextx][nexty] = true;
            count++;
            dfs(graph,visited,nextx,nexty);
        }
    }
}

int main(){
    int n , m;
    cin>> n >> m;
    vector<vector<int>> graph(n,vector<int>(m,0));
    for(int i = 0 ; i < n ; i ++){
        for(int j = 0 ; j < m ; j++){
            cin>>graph[i][j];
        }
    }
    vector<vector<bool>> visited(n,vector<bool>(m,false));
    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && graph[i][j] == 1) {
                count = 1;  // 因为dfs处理下一个节点，所以这里遇到陆地了就先计数，dfs处理接下来的相邻陆地
                visited[i][j] = true;
                dfs(graph, visited, i, j); // 将与其链接的陆地都标记上 true
                result = max(result, count);
            }
        }
    }
    cout<<result<<endl;
}
```
