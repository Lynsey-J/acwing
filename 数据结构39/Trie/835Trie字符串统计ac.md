### 题目描述

维护一个字符串集合，支持两种操作：
“I x”向集合中插入一个字符串x；
“Q x”询问一个字符串在集合中出现了多少次。
共有N个操作，输入的字符串总长度不超过  10^5 ，字符串仅包含小写英文字母。

输入格式
第一行包含整数N，表示操作数。

接下来N行，每行包含一个操作指令，指令为”I x”或”Q x”中的一种。

输出格式
对于每个询问指令”Q x”，都要输出一个整数作为结果，表示x在集合中出现的次数。

每个结果占一行。

数据范围
1≤N≤2∗10^4

### 样例

Input

```
5
I abc
Q abc
Q ab
I ab
Q ab
```

Output

```
1
0
1
```

----------

### 算法
#### Trie

如果暴力，$(10^5) ^ 2$。
如果把每个字符串构成散列，查询复杂度$O(1)$，可能有戏，这里不尝试。

Trie：
初始时只有一个根结点（每个结点都包含26个索引，结点的序号按照创建的顺序定义）。插入一个字符串$s$时，按照$s$中的每个字符，从根结点开始，每个字符往适当的方向向下走一步。同时维护一个$cnt$数组，当某个字符串走到头时，在该结点的序号对应的$cnt$序号处$++$。

#### 时间复杂度

$O(n)$

#### 参考文献

#### C++ 代码

``` cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;

const int N = 100010;

int son[N][26], cnt[N], idx;

void insert(char *s) {
    int p = 0;
    for (int i = 0; s[i]; i++) {
        int c = s[i] - 'a';
        if (!son[p][c]) son[p][c] = ++idx;
        p = son[p][c];
    }
    cnt[p]++;
}

int query(char *s) {
    int p = 0;
    for (int i = 0; s[i]; i++) {
        int c = s[i] - 'a';
        if (!son[p][c]) return 0;
        p = son[p][c];
    }
    return cnt[p];
}

int main() {
    int m;
    scanf("%d", &m);
    while (m--) {
        char op[2], str[100];
        scanf("%s%s", op, str);
        if (op[0] == 'I') {
            insert(str);
        } else {
            printf("%d\n", query(str));
        }
    }
    return 0;
}
```