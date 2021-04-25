### 题目描述

给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少。

输入格式
第一行包含两个整数N和M。
第二行包含一个长度为N的字符串，表示字符串A。
第三行包含一个长度为M的字符串，表示字符串B。
字符串均由小写字母构成。

输出格式
输出一个整数，表示最大长度。

数据范围
1≤N,M≤1000 

### 样例

Input

```
4 5
acbd
abedc
```

Output

```
3
```

----------

### 算法
#### LCS模型dp

序列用$A, B$表示。
状态：$f[i][j]$表示$A[1 \sim i], B[1 \sim j]$的最长公共子序列。
转移：`if (A[i] == B[j])`:$f[i][j] = f[i - 1][j - 1] + 1$
`if (A[i] != B[j])，即A[i]或者B[j]没起到作用`:$f[i][j] = max\{f[i - 1][j], f[i][j - 1]\}$

#### 时间复杂度

$O(n^2)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 1010;

int n, m, f[N][N];
string a, b;

int main() {
    cin >> n >> m;
    cin >> a >> b;
    a = ' ' + a;
    b = ' ' + b;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            if (a[i] == b[j]) f[i][j] = f[i - 1][j - 1] + 1;
            else f[i][j] = max(f[i - 1][j], f[i][j - 1]);
    cout << f[n][m] << endl;
    return 0;
}
```