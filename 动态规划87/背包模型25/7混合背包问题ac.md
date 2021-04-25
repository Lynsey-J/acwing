### 题目描述

有  N  种物品和一个容量是  V  的背包。
物品一共有三类：
第一类物品只能用1次（01背包）；
第二类物品可以用无限次（完全背包）；
第三类物品最多只能用  si  次（多重背包）；
每种体积是  vi ，价值是  wi 。
求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

输入格式
第一行两个整数， N，V ，用空格隔开，分别表示物品种数和背包容积。
接下来有  N  行，每行三个整数  vi,wi,si ，用空格隔开，分别表示第  i  种物品的体积、价值和数量。
si=−1  表示第  i  种物品只能用1次；
si=0  表示第  i  种物品可以用无限次；
si>0  表示第  i  种物品可以使用  si  次；

输出格式
输出一个整数，表示最大价值。

数据范围
0<N,V≤1000 
0<vi,wi≤1000 
−1≤si≤1000

### 样例

Input

```
4 5
1 2 -1
2 4 1
3 4 0
4 5 2
```

Output

```
8
```

----------

### 算法
#### 混合背包

直觉：仿照`5多重背包2`，将使用次数大于1的物品转化成使用次数为1的物品（由于背包容量的限制，无限次 = 1000次）。

yxc：在二层循环之前加一个判断即可，如果是完全背包则从$j = c[i] \sim v$遍历，如果不是就从$j = v \sim c[i]$遍历

#### 时间复杂度

$O(logs \times n \times v)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 10010, V = 1010;

int f[V], com[N], c[N], w[N], n, v;

int main() {
    int t;
    cin >> t >> v;
    for (int i = 0; i < t; i++) {
        int cc, ww, ss;
        cin >> cc >> ww >> ss;
        if (ss > 0) {
            int k = 1;
            while (k <= ss) {
                c[n] = k * cc;
                w[n] = k * ww;
                ss -= k;
                k <<= 1;
                n++;
            }
            if (ss > 0) {
                c[n] = ss * cc;
                w[n] = ss * ww;
                n++;
            }
        } else {
            c[n] = cc;
            w[n] = ww;
            if (ss == 0) com[n] = 1;
            n++;
        }
    }
    for (int i = 0; i < n; i++)
        if (com[i])
            for (int j = c[i]; j <= v; j++)
                f[j] = max(f[j], f[j - c[i]] + w[i]);
        else 
            for (int j = v; j >= c[i]; j--)
                f[j] = max(f[j], f[j - c[i]] + w[i]);
    
    cout << f[v] << endl;
    return 0;
}
```