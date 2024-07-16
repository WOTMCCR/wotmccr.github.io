---
categories: 算法题
title: 2024-07-16 刷题1
date: 2024-07-16
tags:
  - leetcode
  - 并查集
---
## 2024-07-16 刷题
### 108. 冗余连接
09:26
#### 题目描述
给定一个连通无向图，删除一条边，使删除后的图变成一颗树
#### 思路
依次遍历每一条边，当新加入的边的两端都在图中，则说明形成了环，该边就是答案
#### 题解
##### C++
```C++
#include<iostream>
#include<vector>

using namespace std;

int n = 1005;
vector<int> father = vector<int> (n,0);

void init(){
    for(int i = 0 ; i < n ; i++){
        father[i] = i;
    }
}

int find(int u){
    return u == father[u] ? u : father[u] = find(father[u]);
}

bool isSame(int u , int v){
    u = find(u);
    v = find(v);
    return u == v;
}

void join(int u ,int v){
    u = find(u);
    v = find(v);
    if(u == v) return ;
    father[v] = u;
}

int main(){
    int N;
    cin>>N;
    init();
    for(int i = 0 ; i < N ; i++){
        int s,t;
        cin >> s >> t;
        if(isSame(s,t)){
            cout<< s<<" "<< t;
            return 0;
        }
        join(s,t);
    }
}
```

### 109. 冗余连接II
09:40
#### 题目描述
有向图，删除一条边构成树
#### 思路
删除入度为2的边
如果没有入度为2的点，则删除构成环的边
#### 注意⚠️
倒序进行遍历边，记录入度为2的点
#### 题解
##### C++
```C++
#include<iostream>
#include<vector>

using namespace std;
int n;
vector<int> father = vector<int> (1001,0);

void init(){
    for(int i = 1 ; i <= n ; i++){
        father[i] = i;
    }
}

int find(int u){
    return u == father[u] ? u : father[u] = find(father[u]);
}

bool isSame(int u , int v){
    u = find(u);
    v = find(v);
    return u == v;
}

void join(int u ,int v){
    u = find(u);
    v = find(v);
    if(u == v) return ;
    father[v] = u;
}

void getRemoveEdge(const vector<vector<int>> &edges){
    init();
    for(int i = 0 ; i < n ; i++){
        if(isSame(edges[i][0] , edges[i][1])){ //构成了环
            cout << edges[i][0] << " " << edges[i][1];
            return;
        }else{
            join(edges[i][0] , edges[i][1]);
        }
    }
}

bool isTreeAfterRemoveEdge(const vector<vector<int>> &edges , int deleteEdge){
    init();
    for(int i = 0 ; i < n ;i++){
        if(i == deleteEdge) continue;
        if(isSame(edges[i][0],edges[i][1])){ //构成了有向环，一定不是树
            return false;
        }
        join(edges[i][0],edges[i][1]);
    }
    return true;
}

int main(){
    int s,t;
    vector<vector<int>> edges;
    cin>>n;
    vector<int> inDegree(n+1,0);
    for(int i = 0 ; i < n ; i++){
        cin>>s>>t;
        inDegree[t]++;
        edges.push_back({s,t});
    }
    vector<int> vec; //记录入度为2的边
    //倒序查找，优先删除最后出现的边
    for(int i = n - 1 ; i >= 0 ; i--){
        if(inDegree[edges[i][1]] == 2){
            vec.push_back(i);
        }
    }
    
    if(vec.size() > 0){
        //入度为2的点，这条边不是，就是另一条边
        if(isTreeAfterRemoveEdge(edges,vec[0])){
            cout << edges[vec[0]][0] << " " << edges[vec[0]][1];
        }else{
            cout << edges[vec[1]][0] << " " << edges[vec[1]][1];
        }
        return 0;
    }
    
    //没有入度为2的情况，就是有向环
    getRemoveEdge(edges);
}



```
