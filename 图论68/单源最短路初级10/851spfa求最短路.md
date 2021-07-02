### 题目描述

给定一个  n  个点  m  条边的有向图，图中可能存在重边和自环， 边权可能为负数。
请你求出  1  号点到  n  号点的最短距离，如果无法从  1  号点走到  n  号点，则输出 impossible。
数据保证**不存在负权回路**。

输入格式
第一行包含整数  n  和  m 。
接下来  m  行每行包含三个整数  x,y,z ，表示存在一条从点  x  到点  y  的有向边，边长为  z 。

输出格式
输出一个整数，表示  1  号点到  n  号点的最短距离。
如果路径不存在，则输出 impossible。

数据范围
1≤n,m≤105 ,
图中涉及边长绝对值均不超过  10000 。

### 样例

Input

```
3 3
1 2 5
2 3 -3
1 3 4
```

Output

```
2
```

----------

### 算法
#### spfa

基于`bellman-ford`算法优化，只有当$d[i]$的值改变时，才去遍历以结点$i$为箭尾的边，否则没有意义。

证明去查文献。

spfa的$st[i]$表示结点$i$是否在队列中。

#### 时间复杂度

最坏$O(nm)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>

using namespace std;

const int N = 100010, M = N, INF = 0x3f3f3f3f;

int h[N], ne[M], va[M], we[M], idx;
int n, m;
int d[N];
bool st[N];

void insert(int u, int v, int w) {
    ne[++idx] = h[u];
    va[idx] = v;
    we[idx] = w;
    h[u] = idx;
}

void spfa(int u) {
    memset(d, 0x3f, sizeof d);    
    d[u] = 0;
    queue<int> que;
    que.push(u);
    st[u] = true;
    while (!que.empty()) {
        int t = que.front();
        que.pop();
        st[t] = false;
        for (int i = h[t]; i; i = ne[i]) {
            int j = va[i], w = we[i];
            if (d[j] > d[t] + w) {
                d[j] = d[t] + w;
                if (!st[j]) {
                    que.push(j);
                    st[j] = true;
                }
            }
        }
    }
}

int main() {
    scanf("%d%d", &n, &m);
    while (m--) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        insert(u, v, w);
    }
    spfa(1);
    if (d[n] == INF) puts("impossible");
    else printf("%d\n", d[n]);
    return 0;
}
```