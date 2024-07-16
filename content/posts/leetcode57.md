---
categories: 算法题
title: 2024-07-16 刷题6
tags:
  - leetcode
  - 最短路径
  - Floyd
date: 2024-07-16
---
## 2024-07-16 刷题6
### 97. 小明逛公园
16:57
#### 题目描述
求每个起点到终点的最短路径

1、确定dp数组（dp table）以及下标的含义
这里我们用 grid数组来存图，那就把dp数组命名为 grid。
`grid[i][j][k] = m`，表示 节点i 到 节点j 以`[1...k]` 集合为中间节点的最短距离为m。



2、确定递推公式
在上面的分析中我们已经初步感受到了递推的关系。
我们分两种情况：
1. 节点i 到 节点j 的最短路径经过节点k
2. 节点i 到 节点j 的最短路径不经过节点k
对于第一种情况，`grid[i][j][k] = grid[i][k][k - 1] + grid[k][j][k - 1]`
节点i 到 节点k 的最短距离 是不经过节点k，中间节点集合为`[1...k-1]`，所以 表示为`grid[i][k][k - 1]`
节点k 到 节点j 的最短距离 也是不经过节点k，中间节点集合为`[1...k-1]`，所以表示为 `grid[k][j][k - 1]`
第二种情况，`grid[i][j][k] = grid[i][j][k - 1]`
如果节点i 到 节点j的最短距离 不经过节点k，那么 中间节点集合`[1...k-1]`，表示为 `grid[i][j][k - 1]`
因为我们是求最短路，对于这两种情况自然是取最小值。
即： `grid[i][j][k] = min(grid[i][k][k - 1] + grid[k][j][k - 1]， grid[i][j][k - 1])`
#### 复杂度
#### 注意⚠️
#### 题解
##### C++
```C++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<vector<int>> grid(n + 1, vector<int>(n + 1, 10005));  // 因为边的最大距离是10^4

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        grid[p1][p2] = val;
        grid[p2][p1] = val; // 注意这里是双向图

    }
    // 开始 floyd
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                grid[i][j] = min(grid[i][j], grid[i][k] + grid[k][j]);
            }
        }
    }
    // 输出结果
    int z, start, end;
    cin >> z;
    while (z--) {
        cin >> start >> end;
        if (grid[start][end] == 10005) cout << -1 << endl;
        else cout << grid[start][end] << endl;
    }
}

#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<vector<int>> grid(n + 1, vector<int>(n + 1, 10005));  // 因为边的最大距离是10^4

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        grid[p1][p2] = val;
        grid[p2][p1] = val; // 注意这里是双向图

    }
    // 开始 floyd
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                grid[i][j] = min(grid[i][j], grid[i][k] + grid[k][j]);
            }
        }
    }
    // 输出结果
    int z, start, end;
    cin >> z;
    while (z--) {
        cin >> start >> end;
        if (grid[start][end] == 10005) cout << -1 << endl;
        else cout << grid[start][end] << endl;
    }
}
```

### A * 算法精讲 （A star算法）
17:09
#### 题目描述
根据骑士的移动规则，计算从起点到达目标点所需的最短步数。
#### 题解
##### C++
```C++
#include<iostream>
#include<queue>
#include<string.h>
using namespace std;
int moves[1001][1001];
int dir[8][2]={-2,-1,-2,1,-1,2,1,2,2,1,2,-1,1,-2,-1,-2};
int b1, b2;
// F = G + H
// G = 从起点到该节点路径消耗
// H = 该节点到终点的预估消耗

struct Knight{
    int x,y;
    int g,h,f;
    bool operator < (const Knight & k) const{  // 重载运算符， 从小到大排序
     return k.f < f;
    }
};

priority_queue<Knight> que;

int Heuristic(const Knight& k) { // 欧拉距离
    return (k.x - b1) * (k.x - b1) + (k.y - b2) * (k.y - b2); // 统一不开根号，这样可以提高精度
}
void astar(const Knight& k)
{
    Knight cur, next;
	que.push(k);
	while(!que.empty())
	{
		cur=que.top(); que.pop();
		if(cur.x == b1 && cur.y == b2)
		break;
		for(int i = 0; i < 8; i++)
		{
			next.x = cur.x + dir[i][0];
			next.y = cur.y + dir[i][1];
			if(next.x < 1 || next.x > 1000 || next.y < 1 || next.y > 1000)
			continue;
			if(!moves[next.x][next.y])
			{
				moves[next.x][next.y] = moves[cur.x][cur.y] + 1;

                // 开始计算F
				next.g = cur.g + 5; // 统一不开根号，这样可以提高精度，马走日，1 * 1 + 2 * 2 = 5
                next.h = Heuristic(next);
                next.f = next.g + next.h;
                que.push(next);
			}
		}
	}
}

int main()
{
    int n, a1, a2;
    cin >> n;
    while (n--) {
        cin >> a1 >> a2 >> b1 >> b2;
        memset(moves,0,sizeof(moves));
        Knight start;
        start.x = a1;
        start.y = a2;
        start.g = 0;
        start.h = Heuristic(start);
        start.f = start.g + start.h;
		astar(start);
        while(!que.empty()) que.pop(); // 队列清空
		cout << moves[b1][b2] << endl;
	}
	return 0;
}
```


