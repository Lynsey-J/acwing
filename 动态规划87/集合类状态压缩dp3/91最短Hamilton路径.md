### 题目描述

给定一张  n  个点的带权无向图，点从  0∼n−1  标号，求起点  0  到终点  n−1  的最短 Hamilton 路径。
Hamilton 路径的定义是从  0  到  n−1  不重不漏地经过每个点恰好一次。

输入格式
第一行输入整数  n 。
接下来  n  行每行  n  个整数，其中第  i  行第  j  个整数表示点  i  到  j  的距离（记为  a[i,j] ）。
对于任意的  x,y,z ，数据保证  a[x,x]=0，a[x,y]=a[y,x]  并且  **a[x,y]+a[y,z]≥a[x,z]** 。

输出格式
输出一个整数，表示最短 Hamilton 路径的长度。

数据范围
1≤n≤20 
0≤a[i,j]≤10^7

### 样例

Input

```
5
0 2 4 5 1
2 0 6 5 3
4 6 0 8 3
5 5 8 0 5
1 3 3 5 0
```

Output

```
18
```

----------

### 算法
#### 状态压缩dp

注意hamilton路径定义是从$0 \sim n - 1$，不是从任意结点开始。

状态：$f[i][j]$表示走到状态$i$且落在结点$j$的最小路径。
转移：$f[i][j] = min\{f[i \hat{} (1 << j)][k] + a[k][j]\}$, $0 \le k < n$且状态$i \hat{} (1 << j)$包括结点$k$
初始化：

~~`
memset(f, 0x3f, sizeof f);
for (int i = 0; i < n; i++) f[1 << i][i] = 0;
`~~

```
memset(f, 0x3f, sizeof f);
f[1][0]  = 0;
```

答案：

~~`int res = 0;
for (int i = 0; i < n; i++)
res = max(res, f[(1 << n) - 1][i]);`~~

`f[(1 << n) - 1][n - 1];`



补充：受`524愤怒的小鸟`代码的启发，该题状态也可以定义为$f[i]$表示状态$i$的最短路径，求$f[i]$的时候需要遍历$i$的二进制展开式中位为$1$的结点$j$从另一个位为$1$的结点$k$转移。

#### 时间复杂度

$O(n ^ 2 \times 2 ^ {n})$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 22, M = (1 << 20) + 10;

int f[M][N], a[N][N], n;

int main() {
    cin >> n;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> a[i][j];
            
    memset(f, 0x3f, sizeof f);
    f[1][0] = 0;
    for (int i = 1; i < 1 << n; i++)
        for (int j = 0; j < n; j++)
            if (i >> j & 1)
                for (int k = 0; k < n; k++)
                    if ((i ^ (1 << j)) >> k & 1)
                        f[i][j] = min(f[i][j], f[i ^ (1 << j)][k] + a[k][j]);
    cout << f[(1 << n) - 1][n - 1] << endl;
    return 0;
}
```