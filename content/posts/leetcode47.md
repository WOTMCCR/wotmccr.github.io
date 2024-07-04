---
tags:
  - leetcode
  - 图论
title: 2024-07-04刷题
date: 2024-07-04
categories: 算法题
---
## 2024-07-04 刷题
### 98. 所有可达路径
08:40
#### 题目描述
给定一个有n个节点的**有向** 无环图，节点编号从1到n，
找出并返回，所有从节点1到节点n的路径
每条路径以节点编号的列表形式表示
#### 思路
使用邻接矩阵表示图
首先queuing递归函数，参数
```C++
vector<vector<int>> result; // 收集符合条件的路径
vector<int> path; // 0节点到终点的路径
// x：目前遍历的节点
// graph：存当前的图
// n：终点
void dfs (const vector<vector<int>>& graph, int x, int n) {
```

确定终止条件
当目前遍历的节点 为 最后一个节点 n 的时候 就找到了一条 从出发点到终止点的路径。
```C++
// 当前遍历的节点x 到达节点n 
if (x == n) { // 找到符合条件的一条路径
    result.push_back(path);
    return;
}
```

处理目前搜索节点出发的路径
```C++
for (int i = 1; i <= n; i++) { // 遍历节点x链接的所有节点
    if (graph[x][i] == 1) { // 找到 x链接的节点
        path.push_back(i); // 遍历到的节点加入到路径中来
        dfs(graph, i, n); // 进入下一层递归
        path.pop_back(); // 回溯，撤销本节点
    }
}
```

#### 题解
##### C++
```C++

//邻接矩阵
#include<vector>
#include<iostream>
#include<stdio.h>
using namespace std;

vector<vector<int>> result;
vector<int> path;

void dfs(const vector<vector<int>> &graph , int x ,int n){
    if(x == n){
        result.push_back(path);
        return;
    }
    for(int i = 1 ; i <= n ; i++){
        if(graph[x][i] == 1){
            path.push_back(i);
            dfs(graph,i,n);
            path.pop_back();
        }
    }
}

int main(){
    int M,N;
    cin >> N >> M;
    int s = 0 , t = 0;
    //为了节点标号和下标对齐，我们申请 n + 1 * n + 1 
    vector<vector<int>> graph(N + 1 , vector<int>(N + 1 , 0));
    for(int i = 1 ; i <= M ; i++){
        cin>>s>>t;
        graph[s][t] = 1;
    }
    path.push_back(1); // 无论什么路径已经是从0节点出发
    dfs(graph,1 , N);
    
    if(result.size() == 0)  cout<<-1<<endl;
    
    for(int i = 0  ; i < result.size() ;i++){
        for(int j = 0 ; j < result[i].size() ; j++){
            cout<<result[i][j];
            if(j != result[i].size() - 1)
                cout<<" ";
        }
        cout<<endl;
    }
}

```
