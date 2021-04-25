https://www.acwing.com/solution/content/10173/

### 例题

`1010导弹拦截`

`789数的范围`

`187导弹防御系统`：用这个太慢了，手写二分更快。。

### 若数组升序排列

`lower_bound(begin, end, a)`返回数组`[begin, end)`之间第一个大于或等于a的地址，找不到返回end（可以等于所以low）
upper_bound(begin, end, a) 返回数组`[begin, end)`之间第一个大于a的地址，找不到返回end （一定大于所以up）

### 若数组降序排列，可写上比较函数greater<type>()
`lower_bound(begin, end, a, greater<int>())`返回数组`[begin, end)`之间第一个小于或等于a的地址，找不到返回end
`upper_bound(begin, end, a, greater<int>())`返回数组`[begin, end)`之间第一个小于a的地址，找不到返回end

### 非数值数组的情况可选择手写比较函数
如
``` cpp
bool cmp(node a, node b)
{
    if(a.value1 != b.value1) return a.value1 < b.value1;
    else return a.value2 < b.value2;
}
```