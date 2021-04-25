### 题目描述

有  N  种物品和一个容量是  V  的背包，每种物品都有无限件可用。
第  i  种物品的体积是  vi ，价值是  wi 。
求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

输入格式
第一行两个整数， N，V ，用空格隔开，分别表示物品种数和背包容积。
接下来有  N  行，每行两个整数  vi,wi ，用空格隔开，分别表示第  i  种物品的体积和价值。

输出格式
输出一个整数，表示最大价值。

数据范围
0<N,V≤1000 
0<vi,wi≤1000 

### 样例

Input

```
4 5
1 2
2 4
3 4
4 5
```

Output

```
10
```

----------

### 算法
#### 完全背包dp模版

状态：$f[i][j]$表示对于前$i$个物品（每个物品可以取任意次），在体积不超过$j$的情况下的最大价值。
转移：$f[i][j] = max\{f[i - 1][j], f[i - 1][j - c[i]] + w[i], ..., f[i - 1][j - k \times c[i]] + k \times w[i], ...\}, j \ge k \times c[i]$

以上可以用三层循环表示
``` cpp
//在本题中TLE
		cin >> n >> v;
    for (int i = 1; i <= n; i++)
        cin >> c[i] >> w[i];
    for (int i = 1; i <= n; i++)
    	for (int j = 0; j <= v; j++) {
    		if (j >= c[i])
        		for (int k = 0; j >= k * c[i]; k++)
        			f[i][j] = max(f[i][j], f[i - 1][j - k * c[i]] + k * w[i]);
        	else f[i][j] = f[i - 1][j];
    	}
    cout << f[n][v] << endl;
```

优化：
https://www.acwing.com/solution/content/5345/

> $f[i][j] = max\{ f[i-1][j], f[i-1][j-c]+w, f[i-1][j-2 \times c]+2 \times w, f[i-1][j-3 \times c]+3 \times w , ......\}$
> $f[i][j-c] = max\{\ \ \ \ \ \ \ \ \ \ \ f[i-1][j-c]+0, f[i-1][j-2 \times c]+ 1 \times w, f[i-1][j-3 \times c]+2 \times w , ......\}$由上两式，可得出如下递推关系： 
> $f[i][j]=max\{f[i][j-c]+w , f[i-1][j]\}$

故可简化为两层循环：

``` cpp
for (int i = 1; i <= n; i++)
	for (int j = 0; j <= v; j++) //一定要正序
		if (j >= c[i])
			f[i][j] = max(f[i - 1][j], f[i][j - c[i]] + w[i]);
		else
			f[i][j] = f[i - 1][j];
```

优化空间：

省略掉第一维物品。

``` cpp
	for (int i = 1; i <= n; i++)
		for (int j = c; j <= v; j++)
			f[j] = max(f[j], f[j - c] + w);
```

#### 时间复杂度

$O(n \times v)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 1010, V = N;

int f[V], n, v;

int main() {
    cin >> n >> v;
    for (int i = 0; i < n; i++) {
        int c, w;
        cin >> c >> w;
        for (int j = c; j <= v; j++)
            f[j] = max(f[j], f[j - c] + w);
    }
    cout << f[v] << endl;
    return 0;
}
```