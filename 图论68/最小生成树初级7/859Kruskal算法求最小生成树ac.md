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
1≤n≤10^5 ,
1≤m≤2∗10^5 ,
图中涉及边的边权的绝对值均不超过  1000 。

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
#### Kruskal （并查集）

每次选择最短的一条边，如果它连接的两个结点不满足$st[i] == true, st[j] == true$，那么将它加入最小生成树的边集中。

最后边集的大小应该正好是$n - 1$，则存在最小生成树；否则不存在。

#### 时间复杂度

并查集只添加路径压缩优化，均摊复杂度$O(logn)$

$O(m(logm + logn))$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

struct Edge {
    int u, v, w;
    bool operator < (const Edge &e) {
        return w < e.w;
    }
} e[M];

int p[N];
int n, m, res;

int find(int x) {
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

bool kruskal() {
    sort(e, e + m);
    for (int i = 1; i <= n; i++)
        p[i] = i;
    int cnt = 0;
    for (int i = 0; i < m; i++) {
        int u = e[i].u, v = e[i].v, w = e[i].w;
        int pu = find(u), pv = find(v);
        if (pu != pv) {
            p[pu] = pv;
            cnt++;
            res += w;
        }
    }
    return cnt == n - 1;
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++) {
        scanf("%d%d%d", &e[i].u, &e[i].v, &e[i].w);
    }
    if (kruskal()) printf("%d\n", res);
    else puts("impossible");
    return 0;
}
```