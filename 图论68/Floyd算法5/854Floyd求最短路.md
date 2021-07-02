### 题目描述

给定一个  n  个点  m  条边的有向图，图中可能存在重边和自环，边权可能为**负数**。
再给定  k  个询问，每个询问包含两个整数  x  和  y ，表示查询从点  x  到点  y  的最短距离，如果路径不存在，则输出 impossible。

数据保证图中**不存在负权回路**。

输入格式
第一行包含三个整数  n,m,k 。
接下来  m  行，每行包含三个整数  x,y,z ，表示存在一条从点  x  到点  y  的有向边，边长为  z 。
接下来  k  行，每行包含两个整数  x,y ，表示询问点  x  到点  y  的最短距离。

输出格式
共  k  行，每行输出一个整数，表示询问的结果，若询问两点间不存在路径，则输出 impossible。

数据范围
1≤n≤200 ,
1≤k≤n^2 
1≤m≤20000 ,
图中涉及边长绝对值均不超过  10000 。

### 样例

Input

```
3 3 2
1 2 1
2 3 2
1 3 1
2 1
1 3
```

Output

```
impossible
1
```

----------

### 算法
#### floyd

基于动态规划的思想。

状态：$f[k][i][j]$表示从结点$i$到结点$j$，且中间结点只包含前$k$个结点最短路径。
转移：$f[k][i][j] = min\{f[k - 1][i][j], f[k - 1][i][k] + f[k - 1][k][j]\}$
答案：$f[n][i][j]$

初始化：$f[][][] = +\infin$，若$i, j$之间有一条边权为$w$的边，则令$f[0][i][j] = w$；$f[0][i][i] = 0$

空间优化：考虑到$f[k][i][k] = min\{f[k - 1][i][k], f[k - 1][i][k] + f[k - 1][k][k]\}$，又$f[k - 1][k][k] == 0$（不存在负权回路），所以$f[k][i][k] == f[k - 1][i][k]$；同理$f[k][k][j] == f[k - 1][k][j]$。

第$k$轮：$f[i][j] = min\{f[i][j], f[i][k] + f[k][j]\}$

#### 时间复杂度

$O(n ^ 3)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 210, INF = 0x3f3f3f3f;
int dist[N][N];
int n, m, q;

int main() {
    memset(dist, 0x3f, sizeof dist);
    cin >> n >> m >> q;
    for (int i = 1; i <= n; i++) dist[i][i] = 0;
    while (m--) {
        int x, y, z;
        cin >> x >> y >> z;
        dist[x][y] = min(dist[x][y], z);
    }
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
    while (q--) {
        int x, y;
        cin >> x >> y;
        if (dist[x][y] > INF / 2) puts("impossible"); //inf有可能被更新
        else cout << dist[x][y] << endl;
    }
    return 0;
}
```