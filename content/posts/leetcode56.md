---
categories: 算法题
title: 2024-07-16 刷题5
tags:
  - leetcode
  - Floyd
date: 2024-07-16
---
## 2024-07-16 刷题5
### Bellman_ford 队列优化算法（又名SPFA）
16:24
#### 题目描述
使用队列优化
#### 思路
使用队列存储每次更新的部分
#### 复杂度
#### 注意⚠️
其实有环的情况，要看它是 正权回路 还是 负权回路。
题目描述中，已经说了，本题没有 负权回路 。
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <queue>
#include <list>
#include <climits>
using namespace std;

struct Edge { //邻接表
    int to;  // 链接的节点
    int val; // 边的权重

    Edge(int t, int w): to(t), val(w) {}  // 构造函数
};

int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<list<Edge>> grid(n + 1); // 邻接表

    // 将所有边保存起来
    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        // p1 指向 p2，权值为 val
        grid[p1].push_back(Edge(p2, val));
    }
    int start = 1;  // 起点
    int end = n;    // 终点
    
    
    vector<int> minDist(n + 1 , INT_MAX);
    minDist[start] = 0;
    
    queue<int> que;
    que.push(start); // 队列里放入起点
    
    while (!que.empty()) {

        int node = que.front(); que.pop();

        for (Edge edge : grid[node]) {
            int from = node;
            int to = edge.to;
            int value = edge.val;
            if (minDist[to] > minDist[from] + value) { // 开始松弛
                minDist[to] = minDist[from] + value;
                que.push(to);
            }
        }

    }    
    if (minDist[end] == INT_MAX) cout << "unconnected" << endl; // 不能到达终点
    else cout << minDist[end] << endl; // 到达终点最短路径    
}
```


### bellman_ford之判断负权回路
16:31
#### 题目描述
有负回路的最短路径问题
如果没有发现负权回路，则输出一个整数，表示从城市 `1` 到城市 `n` 的最低运输成本（包括政府补贴）。如果该整数是负数，则表示实现了盈利。如果发现了负权回路的存在，则输出 "circle"。如果从城市 1 无法到达城市 n，则输出 "unconnected"。
#### 思路
那么解决本题的 核心思路，就是在 [kama94.城市间货物运输I](https://www.programmercarl.com/kamacoder/kama94.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html) 的基础上，再多松弛一次，看minDist数组 是否发生变化。

#### 复杂度
时间复杂度： O(N * E) , N为节点数量，E为图中边的数量
空间复杂度： O(N) ，即 minDist 数组所开辟的空间
#### 注意⚠️
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

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        // p1 指向 p2，权值为 val
        grid.push_back({p1, p2, val});

    }
    int start = 1;  // 起点
    int end = n;    // 终点

    vector<int> minDist(n + 1 , INT_MAX);
    minDist[start] = 0;
    bool flag = false;
    for (int i = 1; i <= n; i++) { // 这里我们松弛n次，最后一次判断负权回路
        for (vector<int> &side : grid) {
            int from = side[0];
            int to = side[1];
            int price = side[2];
            if (i < n) {
                if (minDist[from] != INT_MAX && minDist[to] > minDist[from] + price) minDist[to] = minDist[from] + price;
            } else { // 多加一次松弛判断负权回路
                if (minDist[from] != INT_MAX && minDist[to] > minDist[from] + price) flag = true;

            }
        }

    }

    if (flag) cout << "circle" << endl;
    else if (minDist[end] == INT_MAX) {
        cout << "unconnected" << endl;
    } else {
        cout << minDist[end] << endl;
    }
}
```

### bellman_ford之单源有限最短路
16:37
#### 题目描述
第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。
接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。
最后一行包含三个正整数，src、dst、和 k，src 和 dst 为城市编号，从 src 到 dst 经过的城市数量限制。

#### 思路
计算 minDist 时候，要基于 对所有边上一次松弛的 minDist 数值才行，所以我们要记录上一次松弛的minDist。
#### 注意⚠️
理论上来说节点3 应该在对所有边第二次松弛的时候才更新。 这因为当时是基于已经计算好的 节点2（minDist[2]）来做计算了。

minDist[2]在计算边：（节点1 -> 节点2）的时候刚刚被赋值为 -1。

这样就造成了一个情况，即：计算minDist数组的时候，基于了本次松弛的 minDist数值，而不是上一次 松弛时候minDist的数值。
所以在每次计算 minDist 时候，要基于 对所有边上一次松弛的 minDist 数值才行，所以我们要记录上一次松弛的minDist。
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
#include <list>
#include <climits>
using namespace std;

int main() {
    int src, dst,k ,p1, p2, val ,m , n;
    
    cin >> n >> m;

    vector<vector<int>> grid;

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        grid.push_back({p1, p2, val});
    }

    cin >> src >> dst >> k;

    vector<int> minDist(n + 1 , INT_MAX);
    minDist[src] = 0;
    vector<int> minDist_copy(n + 1); // 用来记录上一次遍历的结果
    for (int i = 1; i <= k + 1; i++) {
        minDist_copy = minDist; // 获取上一次计算的结果
        for (vector<int> &side : grid) {
            int from = side[0];
            int to = side[1];
            int price = side[2];
            // 注意使用 minDist_copy 来计算 minDist 
            if (minDist_copy[from] != INT_MAX && minDist[to] > minDist_copy[from] + price) {  
                minDist[to] = minDist_copy[from] + price;
            }
        }
    }
    if (minDist[dst] == INT_MAX) cout << "unreachable" << endl; // 不能到达终点
    else cout << minDist[dst] << endl; // 到达终点最短路径

}

```