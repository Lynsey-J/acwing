### 题目描述

给定一个  n  个点  m  条边的有向图，图中可能存在重边和自环，所有边权均为非负值。
请你求出  1  号点到  n  号点的最短距离，如果无法从  1  号点走到  n  号点，则输出  −1 。

输入格式
第一行包含整数  n  和  m 。
接下来  m  行每行包含三个整数  x,y,z ，表示存在一条从点  x  到点  y  的有向边，边长为  z 。

输出格式
输出一个整数，表示  1  号点到  n  号点的最短距离。
如果路径不存在，则输出  −1 。

数据范围
1≤n,m≤1.5×10^5 ,
图中涉及边长均不小于  0 ，且不超过  10000 。

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
#### 堆优化Dijkstra

堆里存储`{d[i], i}`
每次更新完$d[]$都要入堆，堆里的结点可能有重复。

#### 时间复杂度

$O(mlogm?)$

看用什么堆，手写二叉堆是O(elogv)，stl优先队列是O(eloge)，斐波那契堆是O(vlogv+e)，配对堆复杂度玄学

https://tieba.baidu.com/p/5547681010?red_tag=1915746522

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 150010, M = N, INF = 0x3f3f3f3f;
int n, m;
int h[N], va[M], ne[M], we[M], idx;
int d[N];
bool st[N];

void insert(int u, int v, int w) {
    va[++idx] = v, we[idx] = w, ne[idx] = h[u], h[u] = idx;
}

int dijkstra() {
    memset(d, 0x3f, sizeof d);
    d[1] = 0;
    
    priority_queue<PII, vector<PII>, greater<PII> > heap; //小根堆
    heap.push({0, 1});
    while (!heap.empty()) {
        PII t = heap.top();
        heap.pop();
        if (st[t.second]) continue; //堆里可能有重复结点
        st[t.second] = true;
        for (int i = h[t.second]; i; i = ne[i]) {
            int j = va[i], w = we[i];
            if (d[t.second] + w < d[j]) {
            		//更新并入堆
                d[j] = d[t.second] + w;
                heap.push({d[j], j});
            }
        }
    }
    return d[n];
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        insert(u, v, w);        
    }
    
    int t = dijkstra();
    if (t == INF) puts("-1");
    else printf("%d\n", t);
    return 0;
}

```