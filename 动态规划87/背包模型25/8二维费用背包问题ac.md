### 题目描述

有  N  件物品和一个容量是  V  的背包，背包能承受的最大重量是  M 。
每件物品只能用一次。体积是  vi ，重量是  mi ，价值是  wi 。
求解将哪些物品装入背包，可使物品总体积不超过背包容量，总重量不超过背包可承受的最大重量，且价值总和最大。
输出最大价值。

输入格式
第一行两个整数， N，V,M ，用空格隔开，分别表示物品件数、背包容积和背包可承受的最大重量。
接下来有  N  行，每行三个整数  vi,mi,wi ，用空格隔开，分别表示第  i  件物品的体积、重量和价值。

输出格式
输出一个整数，表示最大价值。

数据范围
0<N≤1000 
0<V,M≤100 
0<vi,mi≤100 
0<wi≤1000

### 样例

Input

```
4 5 6
1 2 3
2 4 4
3 4 5
4 5 6

```

Output

```
8
```

----------

### 算法
#### 二维费用背包问题

状态：$f[i][j][k]$表示对于前$i$个物品，体积不超过$j$，质量不超过$k$的最大价值。
考虑是否要往背包中加入第$i$个物品：
转移：$if:j >= c1[i] and k >= c2[i]$, $f[i][j][k] = max\{f[i - 1][j][k], f[i - 1][j - c1[i]][k - c2[i]] + w[i]$
$else$, $f[i][j][k] = f[i - 1][j][k]$

同一维01背包问题：可以优化掉物品这一维的空间。

#### 时间复杂度

$O(n \times v \times m)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 1010, V = 110, M = V;

int f[V][M], c1[N], c2[N], w[N], n, v, m;

int main() {
    cin >> n >> v >> m;
    for (int i = 0; i < n; i++)
        cin >> c1[i] >> c2[i] >> w[i];
    for (int i = 0; i < n; i++)
        for (int j = v; j >= c1[i]; j--)
            for (int k = m; k >= c2[i]; k--)
                f[j][k] = max(f[j][k], f[j - c1[i]][k - c2[i]] + w[i]);
    cout << f[v][m] << endl;
    return 0;
}
```