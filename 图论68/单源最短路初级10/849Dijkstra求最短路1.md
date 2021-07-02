### 题目描述

给定一个  n  个点  m  条边的有向图，图中可能存在重边和自环，所有边权**均为正值**。
请你求出  1  号点到  n  号点的最短距离，如果无法从  1  号点走到  n  号点，则输出  −1 。

输入格式
第一行包含整数  n  和  m 。
接下来  m  行每行包含三个整数  x,y,z ，表示存在一条从点  x  到点  y  的有向边，边长为  z 。

输出格式
输出一个整数，表示  1  号点到  n  号点的最短距离。
如果路径不存在，则输出  −1 。

数据范围
1≤n≤500 ,
1≤m≤10^5 ,
图中涉及边长均不超过10000。

### 样例

Input

```
3 3
1 2 2
2 3 1
1 3 4
```

Output

```
3
```

----------

### 算法
#### dijkstra模版

证明：参考算法导论

#### 时间复杂度

$O(n ^ 2)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int g[N][N], d[N];
bool st[N];
int n, m;

void dijkstra() {
    memset(d, 0x3f, sizeof d);
    d[1] = 0;
    for (int i = 0; i < n; i++) { //n轮
        int t = 0;
        for (int j = 1; j <= n; j++) //找出未被访问的结点中d值最小的那个
            if (!st[j] && (!t || d[j] < d[t]))
                t = j;
        st[t] = true;
        for (int j = 1; j <= n; j++) //用当前结点更新其余结点
            d[j] = min(d[j], d[t] + g[t][j]);
    }
}

int main() {
    scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof g);
    for (int i = 1; i <= n; i++) g[i][i] = 0;
    while (m--) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        g[u][v] = min(g[u][v], w);
    }
    
    dijkstra();
    if (d[n] == INF) puts("-1");
    else printf("%d\n", d[n]);
    return 0;
}
```