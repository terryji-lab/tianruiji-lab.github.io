# P3128 [USACO15DEC] Max Flow P

## 题目描述

Farmer John 在他的谷仓中安装了 $N-1$ 条管道，用于在 $N$ 个牛棚之间运输牛奶（ $2 \leq N \leq 50,000$ ），牛棚方便地编号为 $1 \ldots N$。每条管道连接一对牛棚，所有牛棚通过这些管道相互连接。

FJ 正在 $K$ 对牛棚之间泵送牛奶（ $1 \leq K \leq 100,000$ ）。对于第 $i$ 对牛棚，你被告知两个牛棚 $s_i$ 和 $t_i$，这是牛奶以单位速率泵送的路径的端点。FJ 担心某些牛棚可能会因为过多的牛奶通过它们而不堪重负，因为一个牛棚可能会作为许多泵送路径的中转站。请帮助他确定通过任何一个牛棚的最大牛奶量。如果牛奶沿着从 $s_i$ 到 $t_i$ 的路径泵送，那么它将被计入端点牛棚 $s_i$ 和 $t_i$，以及它们之间路径上的所有牛棚。

## 输入格式

输入的第一行包含 $N$ 和 $K$。

接下来的 $N-1$ 行每行包含两个整数 $x$ 和 $y$（$ x \ne y $），描述连接牛棚 $x$ 和 $y$ 的管道。

接下来的 $K$ 行每行包含两个整数 $s$ 和 $t$，描述牛奶泵送路径的端点牛棚。

## 输出格式

输出一个整数，表示通过谷仓中任何一个牛棚的最大牛奶量。

## 输入输出样例 #1

### 输入 #1

```
5 10
3 4
1 5
4 2
5 4
5 4
5 4
3 5
4 3
4 3
1 3
3 5
5 4
1 5
3 4
```

### 输出 #1

```
9
```

## 说明/提示

$2 \le N \le 5 \times 10^4,1 \le K \le 10^5$。
## 分析
树上点差分模板题，代码如下
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5e4+10;
vector<int> e[N];
int n,k,ans;
int diff[N];
int dep[N];
int st[N][20];
void dfs(int u,int fa)
{
    dep[u]=dep[fa]+1;
    st[u][0]=fa;
    for(int i=1;i<=15;i++)
    {
        st[u][i]=st[st[u][i-1]][i-1];
    }
    for(auto&x:e[u])
    {
        if(x!=fa)dfs(x,u);
    }
}
int lca(int u,int v)
{
    if(dep[u]<dep[v])swap(u,v);
    for(int i=15;i>=0;i--)
    {
        if(dep[st[u][i]]>=dep[v])u=st[u][i];
    }
    if(u==v)return v;
    for(int i=15;i>=0;i--)
    {
        if(st[u][i]!=st[v][i])u=st[u][i],v=st[v][i];
    }
    return st[u][0];
}
void dfs2(int u,int fa)
{
    for(auto&x:e[u])
    {
        if(x==fa)continue;
        dfs2(x,u);
        diff[u]+=diff[x];
    }
    ans=max(ans,diff[u]);
}
int main()
{
    cin>>n>>k;
    for(int i=1;i<n;i++)
    {
        int u,v;
        cin>>u>>v;
        e[u].push_back(v);
        e[v].push_back(u);
    }
    dfs(1,0);
    for(int i=1;i<=k;i++)
    {
        int x,y;
        cin>>x>>y;
        diff[x]+=1;
        diff[y]+=1;
        int p=lca(x,y);
        diff[p]-=1;
        diff[st[p][0]]-=1;
    }
    dfs2(1,0);
    cout<<ans;
    return 0;
}
```