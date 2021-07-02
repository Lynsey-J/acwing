### 题目描述

Windy 定义了一种 Windy 数：不含前导零且相邻两个数字之差至少为  2  的正整数被称为 Windy 数。

Windy 想知道，在  A  和  B  之间，包括  A  和  B ，总共有多少个 Windy 数？

输入格式
共一行，包含两个整数  A  和  B 。

输出格式
输出一个整数，表示答案。

数据范围
1≤A≤B≤2×10^9

### 样例

Input1

```
1 10
```

Output1

```
9
```

Input2

```
25 50
```

Output2

```
20
```

----------

### 算法
#### 数位dp

`dfs(int pos, int last, bool lead, bool limit)`，如果$lead == true$则不必考虑当前位与$last$须相差至少为2。

#### 时间复杂度

$O(?)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 33;

int num[N], f[N][10];

int dfs(int pos, int last, bool lead, bool limit) {
    if (pos == 0) return 1;
    if (!lead && !limit && f[pos][last]) return f[pos][last];
    int top = limit ? num[pos] : 9;
    int ans = 0;
    for (int i = 0; i <= top; i++) {
        if (!lead && abs(i - last) < 2) continue;
        ans += dfs(pos - 1, i, lead && i == 0, limit && i == top);
    }
    if (!lead && !limit) f[pos][last] = ans;
    return ans;
}

int solve(int a) {
    int len = 0;
    while (a) {
        num[++len] = a % 10;
        a /= 10;
    }
    return dfs(len, 0, true, true);
}

int main() {
    int a, b;
    cin >> a >> b;
    cout << solve(b) - solve(a - 1) << endl;
    return 0;
}
```