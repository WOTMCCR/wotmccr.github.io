---
categories: 算法题
title: 2024-07-06刷题
tags:
  - leetcode
  - 并查集
  - 图论
date: 2024-07-06
---
## 2024-07-06 刷题
### 并查集

并查集常用来解决连通性问题

需要判断两个元素是否在同一个集合的时候，就需要使用并查集

并查集主要有两个功能：
- 将两个元素添加到一个集合中
- 判断两个元素在不在同一个集合

#### 原理
我们将三个元素A，B，C （分别是数字）放在同一个集合，其实就是将三个元素连通在一起
只需要用一个一维数组来表示，即：`father[A] = B，father[B] = C` 这样就表述 A 与 B 与 C连通了（有向连通图）。
```C++
// 将v，u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根
    if (u == v) return; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    father[v] = u;
}
```

这里要讲到寻根思路，只要 A ，B，C 在同一个根下就是同一个集合。
一层一层的寻找
```C++
// 并查集里寻根的过程
int find(int u) {
    if (u == father[u]) return u; // 如果根就是自己，直接返回
    else return find(father[u]); // 如果根不是自己，就根据数组下标一层一层向下找
}
```

初始化
father数组初始化的时候要 father[i] = i，默认自己指向自己。
```C++
// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}
```

判断是否在同一个集合里
```C++
// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}
```

#### 路径压缩
在实现 find 函数的过程中，我们知道，通过递归的方式，不断获取father数组下标对应的数值，最终找到这个集合的根。

如果这棵多叉树高度很深的话，每次find函数 去寻找根的过程就要递归很多次。

我们的目的只需要知道这些节点在同一个根下就可以，所以对这棵多叉树的构造只需要这样就可以了，如图：
![[Pasted image 20240706144248.png]]
如果我们想达到这样的效果，就需要 **路径压缩**，将非根节点的所有节点直接指向根节点
只需要在递归的过程中，让 `father[u]`接住 递归函数 `find(father[u])`的返回结果。

```C++
// 并查集里寻根的过程
int find(int u) {
    if (u == father[u]) return u;
    else return father[u] = find(father[u]); // 路径压缩
}
```

#### 代码模版
```C++
int n = 1005; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, 0); // C++里的一种数组结构

// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}
// 并查集里寻根的过程
int find(int u) {
    return u == father[u] ? u : father[u] = find(father[u]); // 路径压缩
}

// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

// 将v->u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根
    if (u == v) return ; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    father[v] = u;
}
```

#### rank合并
在join函数中一定是rank小的树并到rank大的树中，可以保证合成的树rank最小，降低在树上查询的路径长度

```C++
int n = 1005; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, 0); // C++里的一种数组结构
vector<int> rank = vector<int> (n, 1); // 初始每棵树的高度都为1

// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
        rank[i] = 1; // 也可以不写
    }
}
// 并查集里寻根的过程
int find(int u) {
    return u == father[u] ? u : find(father[u]);// 注意这里不做路径压缩
}

// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

// 将v->u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根

    if (rank[u] <= rank[v]) father[u] = v; // rank小的树合入到rank大的树
    else father[v] = u;

    if (rank[u] == rank[v] && u != v) rank[v]++; // 如果两棵树高度相同，则v的高度+1，因为上面 if (rank[u] <= rank[v]) father[u] = v; 注意是 <=
}
```
**所以这里推荐大家直接使用路径压缩的并查集模板就好**，但按秩合并的优化思路我依然给大家讲清楚，有助于更深一步理解并查集的优化过程。



### 107. 寻找存在的路径
14:26
#### 题目描述
给定一个n个节点的无向图中，节点编号从1到n
判断是否有一条从节点source出发到节点destination的路径存在
#### 思路
将所有边添加到并查集中
判断节点source出发到节点destination是否用共同的father
#### 题解
##### C++
```C++
#include<iostream>
#include<vector>

using namespace std;

int n = 105;
vector<int> father = vector<int>(n,0);

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

void join(int u , int v){
    u = find(u);
    v = find(v);
    if(u == v) return; //已经在集合中
    father[v] = u;
}
int main(){
    int N,M,s,t;
    cin>>N>>M;
    init();
    
    for(int i = 0 ; i < M ; i++){
        cin>>s>>t;
        join(s,t);
    }
    cin>>s>>t;
    if(isSame(s,t))
        cout<<1<<endl;
    else
        cout<<0<<endl;
}
```

