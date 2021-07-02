### 题目描述

给定一个  n  个点  m  条边的无向图，图中可能存在重边和自环，**边权可能为负数**。
求最小生成树的树边权重之和，如果最小生成树不存在则输出 impossible。
给定一张边带权的无向图  G=(V,E) ，其中  V  表示图中点的集合， E  表示图中边的集合， n=|V| ， m=|E| 。
由  V  中的全部  n  个顶点和  E  中  n−1  条边构成的无向连通子图被称为  G  的一棵生成树，其中边的权值之和最小的生成树被称为无向图  G  的最小生成树。

输入格式
第一行包含两个整数  n  和  m 。
接下来  m  行，每行包含三个整数  u,v,w ，表示点  u  和点  v  之间存在一条权值为  w  的边。

输出格式
共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 impossible。

数据范围
1≤n≤500 ,
1≤m≤10^5 ,
图中涉及边的边权的绝对值均不超过  10000 。

### 样例

Input

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

Output

```
6
```

----------

### 算法
#### Prim

图不连通时，显然不存在最小生成树。
无向图连通时，一定存在最小生成树。

Prim算法类似Dijkstra算法将所有点分成两个集合$S, U$，不同的是Prim算法集合$S$扩充新结点的时候是选一条$S, U$之间最“短”的边，Dijkstra算法在$U$中选一个拥有最小距离的结点。

Prim算法**可以处理负权边**。
如果Prim算法可以处理正权图，那么如果某张图存在负权边，可以给所有边加上一个常数让它变成正权图。。

反证法：当不选择$S, U$集合间最短的边$w_s$，而选择另一条更长的边$w_l$，在最后构成的生成树中添加边$w_s$，构成一个环，在环中去掉边$w_l$，必然更优。。。

严谨的证明去查算法导论。

#### 时间复杂度

邻接矩阵$O(n ^ 2)$

邻接表$O(max(n^ 2, m))$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 510, M = 2e5 + 10, INF = 0x3f3f3f3f;

int h[N], ne[M], va[M], we[M], idx;
int n, m, tw[N], res;
bool st[N];

void insert(int u, int v, int w) {
    ne[++idx] = h[u];
    va[idx] = v;
    we[idx] = w;
    h[u] = idx;
}

bool prim() {
    memset(tw, 0x3f, sizeof tw);
    tw[1] = 0;
    for (int i = 0; i < n; i++) {
        int t = 0;
        for (int j = 1; j <= n; j++)
            if (!st[j] && (!t || tw[t] > tw[j]))
                t = j;
        st[t] = true;
        if (tw[t] == INF) return false;
        res += tw[t];
        for (int j = h[t]; j; j = ne[j]) {
            int son = va[j], w = we[j];
            tw[son] = min(tw[son], w);
        }
    }
    return true;
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        insert(u, v, w);
        insert(v, u, w);
    }
    if (prim()) printf("%d\n", res);
    else puts("impossible");
    return 0;
}
```