# P1879 [USACO06NOV] Corn Fields G

## 题目描述

农场主 $\rm John$ 新买了一块长方形的新牧场，这块牧场被划分成 $M$ 行 $N$ 列 $(1 \le M \le 12, 1 \le  N \le 12)$，每一格都是一块正方形的土地。 $\rm John$ 打算在牧场上的某几格里种上美味的草，供他的奶牛们享用。

遗憾的是，有些土地相当贫瘠，不能用来种草。并且，奶牛们喜欢独占一块草地的感觉，于是 $\rm John$ 不会选择两块相邻的土地，也就是说，没有哪两块草地有公共边。

$\rm John$ 想知道，如果不考虑草地的总块数，那么，一共有多少种种植方案可供他选择？（当然，把新牧场完全荒废也是一种方案）

## 输入格式

第一行：两个整数 $M$ 和 $N$，用空格隔开。

第 $2$ 到第 $M+1$ 行：每行包含 $N$ 个用空格隔开的整数，描述了每块土地的状态。第 $i+1$ 行描述了第 $i$ 行的土地，所有整数均为 $0$ 或 $1$ ，是 $1$ 的话，表示这块土地足够肥沃，$0$ 则表示这块土地不适合种草。

## 输出格式

一个整数，即牧场分配总方案数除以 $10^8$ 的余数。

## 输入输出样例 #1

### 输入 #1

```
2 3
1 1 1
0 1 0
```

### 输出 #1

```
9

```

## 分析
又来做状压dp了，有点手感了。这道题的难点就在于如果进行单行合法性判断和多行合法性判断。
### dp数组含义
``dp[mask][row]``表示在第``row``行的``mask``状态下的方案数
### 初始化
我们对第一行进行初始化，第一行中合法的mask都初始化为``1``
### 递推公式
第``row``行的``mask``状态的方案总数等于``row-1``行不与当前``mask``冲突的方案总数之和。最后统计最后一行的方案总数，就是答案。
代码如下，请终点关注mask的合法性判断部分。
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 15;
const int MOD = 100000000;

int m, n;
int grass[N];
int dp[1 << 13][N];

int main() {
    cin >> m >> n;

    for (int i = 1; i <= m; i++) {
        for (int j = n; j >= 1; j--) {
            int x;
            cin >> x;
            if (x) grass[i] |= (1 << j);//首先读入草地的状态，所有的状态都不能和草地状态冲突
        }
    }

    for (int mask = 0; mask < (1 << (n + 1)); mask++) {//从0开始枚举，因为空草地也是一种方案
        if (mask & (mask >> 1)) continue;//这是判断行内合法，右移一位后，如果有相邻的1结果就会不是0
        if (mask & (~grass[1])) continue;//这是判断是否与草地合法
        dp[mask][1] = 1;//给合法草地初始化
    }

    // DP 转移
    for (int row = 2; row <= m; row++) {//枚举行
        for (int mask = 0; mask < (1 << (n + 1)); mask++) {//枚举状态
            if (mask & (mask >> 1)) continue;//行内合法性判断
            if (mask & (~grass[row])) continue;//草地合法性判断
            for (int mask2 = 0; mask2 < (1 << (n + 1)); mask2++) {//枚举上一行的状态
                if (mask2 & (mask2 >> 1)) continue;
                if (mask2 & (~grass[row - 1])) continue;//合法性判断
                if (!(mask2 & mask)) {//行于行之间合法性判断，如果合法就加上上一行的方案数
                    dp[mask][row] = (dp[mask][row] + dp[mask2][row - 1]) % MOD;
                }
            }
        }
    }

    int ans = 0;
    for (int mask = 0; mask < (1 << (n + 1)); mask++) {
        if (mask & (mask >> 1)) continue;
        if (mask & (~grass[m])) continue;
        ans = (ans + dp[mask][m]) % MOD;//统计答案
    }

    cout << ans << endl;
    return 0;
}
```