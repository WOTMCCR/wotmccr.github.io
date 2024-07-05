---
categories: 算法题
title: 2024-07-05刷题
tags:
  - leetcode
  - 图论
date: 2024-07-05
---
## 2024-07-05 刷题
### 110. 字符串接龙
16:50
#### 题目描述
从字符串beginStr到endStr的转换，最后输出，需要的最短转换序列中的字符串数量。
每次更改一个字符，所涉及到的所有中间字符，必须要从字典中选取。
所以类似一个选取路径到达endStr的过程。
#### 思路
是一个寻找最短路径的问题
**这里无向图求最短路，广搜最为合适，广搜只要搜到了终点，那么一定是最短的路径**。因为广搜就是以起点中心向四周扩散的搜索。
#### 复杂度
#### 注意⚠️
- 本题是一个无向图，需要用标记位，标记着节点是否走过，否则就会死循环！
- 使用set来检查字符串是否出现在字符串集合里更快一些

遍历26个字母需要使用字符
`newWord[i] = j + 'a';`
#### 题解
##### C++
```C++
#include<iostream>
#include <vector>
#include <string>
#include <unordered_set>
#include <unordered_map>
#include <queue>
using namespace std;




int main(){
    int n;
    cin>>n;
    string beginStr,endStr ,str;
    cin>>beginStr>>endStr;
    
    unordered_set<string> strList;
    for(int i = 0 ; i < n ; i++){
        cin >> str;
        strList.insert(str);
    }
    
    // 记录strSet里的字符串是否被访问过，同时记录路径长度
    unordered_map<string,int> visitedMap;// <记录的字符串，路径长度>
    
    //初始化队列用于广度优先搜索
    queue<string> que;
    que.push(beginStr);
    
    visitedMap.insert(pair<string,int>(beginStr,1));
    
    //广度优先搜索
    while(!que.empty()){
        string word = que.front();
        que.pop();
        int path = visitedMap[word];
        
        // 开始在这个str中，挨个字符去替换
        for(int i = 0 ; i < word.size() ; i++){
            string newWord = word;
            for(int j = 0 ; j < 26 ; j++){
                newWord[i] = j + 'a';
                
                //如果到达结尾
                if(newWord == endStr){
                    cout<< path + 1 << endl;
                    return 0;
                }
                
                // 字符串集合里出现了newWord，并且newWord没有被访问过
                if(strList.find(newWord) != strList.end() 
                    && visitedMap.find(newWord) == visitedMap.end()){
                        //标记访问
                        visitedMap.insert(pair<string,int>(newWord,path+1));
                        //添加到que
                        que.push(newWord);
                    }
            }
        }
    }
    
    cout<<0<<endl;
}

```

### 105. 有向图的完全可达性
18:07
#### 题目描述
给定一个有向图，包含 N 个节点，节点编号分别为 1，2，...，N。现从 1 号节点开始，如果可以从 1 号节点的边可以到达任何节点，则输出 1，否则输出 -1。
#### 思路
使用深度优先搜索，看是否可以遍历
#### 注意⚠️
使用一个visited来标记是否访问到
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <list>
using namespace std;

//dfs
//需要传入地图，需要知道当前我们拿到的key，以至于去下一个房间。
void dfs(const vector<list<int>> &graph , vector<bool> &visited , int key){
    //访问过
    if(visited[key])
        return;
        
    visited[key] = true;
    list<int> keys = graph[key];
    
    for(int k : keys){
        dfs(graph,visited,k);
    }
    
}

int main(){
    int n,k,s,t;
    cin>>n>>k;
    //使用邻接表的方法，表示,编号从1开始
    vector<list<int>> graph(n+1);
    
    for(int i = 0 ; i < k ; i++){
        cin>>s>>t;
        graph[s].push_back(t);
    }
    
    //判断是否访问
    vector<bool> visited(n+1,false);
    
    //dfs
    dfs(graph,visited,1);
    
    
    for(int i = 1 ; i <= n ; i++){
        if(visited[i] == false){
            cout<<-1<<endl;
            return 0;
        }
    }
    cout<<1<<endl;
}
```

### 106. 岛屿的周长
18:35
#### 题目描述
在矩阵中恰好拥有一个岛屿，假设组成岛屿的陆地边长都为 1，请计算岛屿的周长。
#### 思路
遍历岛屿，如果到达边界就+1，使用一个visited来记录遍历的部分。
#### 题解
##### C++
```C++
//使用dfs
#include<iostream>
#include<vector>
using namespace std;

int s = 0;
int dir[4][2] = {0,1,1,0,-1,0,0,-1};
void dfs(const vector<vector<int>> &graph , vector<vector<bool>> &visited , int x ,int y){
    if(visited[x][y]){
        return;
    }
    visited[x][y] = true;
    
    for(int i = 0 ; i < 4 ; i++){
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if (nextx < 0 || nextx >= graph.size() || nexty < 0 || nexty >= graph[0].size()) 
        {
            s += 1;
            continue; 
        }
        if(graph[nextx][nexty] != 0){
            dfs(graph,visited,nextx,nexty);
        }else{
            s += 1;
        }
    }
}

int main(){
    int n,m;
    cin>>n>>m;
    //使用邻接矩阵表示
    vector<vector<int>> graph(n,vector<int>(m,0));
    for(int i = 0 ; i < n ; i++){
        for(int j = 0 ; j < m ; j++){
            cin>>graph[i][j];
        }
    }
    vector<vector<bool>> visited(n,vector<bool>(m,false));
    
    for(int i = 0 ; i < n ; i++){
        for(int j = 0 ; j < m ; j++){
            if(graph[i][j] != 0){
                dfs(graph,visited,i,j);
                cout<<s<<endl;
                return 0;
            }
        }
    }    
}

//遍历图中所有节点
#include <iostream>
#include <vector>
using namespace std;
int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    int direction[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};
    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                for (int k = 0; k < 4; k++) {       // 上下左右四个方向
                    int x = i + direction[k][0];
                    int y = j + direction[k][1];    // 计算周边坐标x,y
                    if (x < 0                       // x在边界上
                            || x >= grid.size()     // x在边界上
                            || y < 0                // y在边界上
                            || y >= grid[0].size()  // y在边界上
                            || grid[x][y] == 0) {   // x,y位置是水域
                        result++;
                    }
                }
            }
        }
    }
    cout << result << endl;

}
```






