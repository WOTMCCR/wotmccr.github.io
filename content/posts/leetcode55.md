---
categories: 算法题
title: 2024-07-16 刷题4
tags:
  - leetcode
  - 最短路径
  - Floyd
date: 2024-07-16
---
## 2024-07-16 刷题4
### 卡码网：47. 参加科学大会
14:53
#### 题目描述
找最短路径
#### 思路
使用堆优化的Dijkstra算法
直接把 边（带权值）加入到 小顶堆（利用堆来自动排序），那么每次我们从 堆顶里 取出 边 自然就是 距离源点最近的节点所在的边。

邻接表用 数组+链表 来表示，代码如下：（C++中 vector 为数组，list 为链表， 定义了 n+1 这么大的数组空间）
```C++
vector<list<pair<int,int>>> grid(n + 1);
```

我们要选择距离源点近的节点（即：该边的权值最小），所以 我们需要一个 小顶堆 来帮我们对边的权值排序，每次从小顶堆堆顶 取边就是权值最小的边。
C++定义小顶堆，可以用优先级队列实现，代码如下：
```C++
// 小顶堆
class mycomparison {
public:
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        return lhs.second > rhs.second;
    }
};
// 优先队列中存放 pair<节点编号，源点到该节点的权值> 
priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pq;
```

#### 复杂度
时间复杂度：O(ElogE) E 为边的数量
空间复杂度：O(N + E) N 为节点的数量
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <list>
#include <queue>
#include <climits>
using namespace std; 
// 小顶堆
class mycomparison {
public:
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        return lhs.second > rhs.second;
    }
};
// 定义一个结构体来表示带权重的边
struct Edge {
    int to;  // 邻接顶点
    int val; // 边的权重

    Edge(int t, int w): to(t), val(w) {}  // 构造函数
};

int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<list<Edge>> grid(n + 1);

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val; 
        // p1 指向 p2，权值为 val
        grid[p1].push_back(Edge(p2, val));

    }

    int start = 1;  // 起点
    int end = n;    // 终点

    // 存储从源点到每个节点的最短距离
    std::vector<int> minDist(n + 1, INT_MAX);

    // 记录顶点是否被访问过
    std::vector<bool> visited(n + 1, false); 
    
    // 优先队列中存放 pair<节点，源点到该节点的权值>
    priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pq;


    // 初始化队列，源点到源点的距离为0，所以初始为0
    pq.push(pair<int, int>(start, 0)); 
    
    minDist[start] = 0;  // 起始点到自身的距离为0

    while (!pq.empty()) {
        // 1. 第一步，选源点到哪个节点近且该节点未被访问过 （通过优先级队列来实现）
        // <节点， 源点到该节点的距离>
        pair<int, int> cur = pq.top(); pq.pop();

        if (visited[cur.first]) continue;

        // 2. 第二步，该最近节点被标记访问过
        visited[cur.first] = true;

        // 3. 第三步，更新非访问节点到源点的距离（即更新minDist数组）
        for (Edge edge : grid[cur.first]) { // 遍历 cur指向的节点，cur指向的节点为 edge
            // cur指向的节点edge.to，这条边的权值为 edge.val
            if (!visited[edge.to] && minDist[cur.first] + edge.val < minDist[edge.to]) { // 更新minDist
                minDist[edge.to] = minDist[cur.first] + edge.val;
                pq.push(pair<int, int>(edge.to, minDist[edge.to]));
            }
        }

    }

    if (minDist[end] == INT_MAX) cout << -1 << endl; // 不能到达终点
    else cout << minDist[end] << endl; // 到达终点最短路径
}
```




### 94. 城市间货物运输 I
15:57
#### 题目描述
最短路径，可能有负值
#### 思路

#### 复杂度
时间复杂度： O(N * E) , N为节点数量，E为图中边的数量
空间复杂度： O(N) ，即 minDist 数组所开辟的空间
#### 注意⚠️
节点数量为n，那么起点到终点，最多是 n-1 条边相连。
那么无论图是什么样的，边是什么样的顺序，我们对所有边松弛 n-1 次 就一定能得到 起点到达 终点的最短距离。
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <list>
#include <climits>
using namespace std;

int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<vector<int>> grid;

    // 将所有边保存起来
    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        // p1 指向 p2，权值为 val
        grid.push_back({p1, p2, val});

    }
    int start = 1;  // 起点
    int end = n;    // 终点

    vector<int> minDist(n + 1 , INT_MAX);
    minDist[start] = 0;
    for (int i = 1; i < n; i++) { // 对所有边 松弛 n-1 次
        for (vector<int> &side : grid) { // 每一次松弛，都是对所有边进行松弛
            int from = side[0]; // 边的出发点
            int to = side[1]; // 边的到达点
            int price = side[2]; // 边的权值
            // 松弛操作 
            // minDist[from] != INT_MAX 防止从未计算过的节点出发
            if (minDist[from] != INT_MAX && minDist[to] > minDist[from] + price) { 
                minDist[to] = minDist[from] + price;  
            }
        }
    }
    if (minDist[end] == INT_MAX) cout << "unconnected" << endl; // 不能到达终点
    else cout << minDist[end] << endl; // 到达终点最短路径

}
```

