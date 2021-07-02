### 题目描述

给定一个  n  个点  m  条边的有向图，图中可能存在重边和自环， 边权可能为负数。
请你判断图中是否存在负权回路。

输入格式
第一行包含整数  n  和  m 。
接下来  m  行每行包含三个整数  x,y,z ，表示存在一条从点  x  到点  y  的有向边，边长为  z 。

输出格式
如果图中存在负权回路，则输出 Yes，否则输出 No。

数据范围
1≤n≤2000 ,
1≤m≤10000 ,
图中涉及边长绝对值均不超过  10000 。

### 样例

Input

```
3 3
1 2 -1
2 3 4
3 1 -4
```

Output

```
Yes
```

----------

### 算法
#### spfa

考虑一个虚拟原点（图不一定连通），到所有结点连一条权为0的边。
如果虚拟原点到某个结点的最短路径上的边数$\ge n$（不包括从虚拟原点出来的边），那么肯定存在负环。

#### 时间复杂度

$O(nm)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>

using namespace std;

const int N = 2010, M = 10010;

int n, m;
int h[N], ne[M], va[M], we[M], idx;
int d[N], cnt[N];
bool st[N];

void insert(int u, int v, int w) {
    ne[++idx] = h[u];
    va[idx] = v;
    we[idx] = w;
    h[u] = idx;
}

bool spfa() {
    queue<int> q;
    //考虑存在某一个虚拟原点，到所有结点的边权不重要
    for (int i = 1; i <= n; i++) {
        q.push(i);
        st[i] = true;
    }
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i; i = ne[i]) {
            int j = va[i], w = we[i];
            if (d[j] > d[t] + w) {
                d[j] = d[t] + w;
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true; //存在负环
                if (!st[j]) {
                    st[j] = true;
                    q.push(j);
                }
            }
        }
    }
    return false; //无负环
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        insert(u, v, w);
    }
    if (spfa()) puts("Yes");
    else puts("No");
    return 0;
}
```