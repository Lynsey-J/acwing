复杂度？？？

记忆化搜索

``` cpp
int num[N]; //num数字存某一个数字，低位存个位数
int f[N][...]; //f[i][...]表示低i位数字, ...与各种限制和变量对应

//设num[1] = 0, num[2] = 1, num[3] = 2, num[4] = 3表示数字3210
int dfs(int pos/*位数*/,int .../*传的各种限制和变量*/,bool lead/*该位是否是最高位（处理有前导0的情况）*/, bool limit/*该位是否有上限*/) { //如3xxx有百位上限，32xx十位有上限，2xxx百位无上限
    if(pos == 0)return 1;//一般返回1
    if(!limit && f[pos][...] != -1) return f[pos][...];//搜过且该位没有上限就返回搜过的答案
    int top = limit ? num[pos] : 9;//确定枚举上限
    int ans = 0; 
    for(int i = 0; i <= top; i++) {
        if(...)//限制条件 
					ans += dfs(pos - 1,..., i == 0 && lead, limit && (i == top));//如果本位已经枚举到上限就把上限往后传，本位是0则把lead往后传
    } 
    if(!limit) f[pos][...] = ans;//因为如果枚举到上限则答案并不是这一位上所有的和，所以就不更新 
    return ans; 
}

int solve(int x)
{
    int len=0;
    while(x) {
        num[++len] = x % 10;
        x /= 10;
    }
    return dfs(len,..., true, true);
} 
int main() {
  	memset(f, -1, sizeof f);
    int a, b;
    scanf("%d %d", &a, &b);
    printf("%d", solve(b) - solve(a-1));//求答案可以转为求[1,b]-[1,a) 
} 
```