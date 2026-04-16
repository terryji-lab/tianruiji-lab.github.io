# 最近公共祖先LCA
# 算法简介
给定一棵树和树上的两个节点，要求出这两个节点最近的公共祖先。一般我们会采用递归和类似于ST表的方式去处理这种问题。ST表的二进制优化可以大大提高程序的运行速度。
## 状态定义
和ST表相同，我们要有一个``st[i][j]``数组，它表示：**从下标``i``开始，向上走``2^j``条边的节点**。显然对于``st[i][0]``，**表示的就是它的父节点**。
## 状态转移
我们使用dfs来进行打表。**st表表示的节点如果超出了树根的范围，就将其视为``0``节点（哨兵）**。**显然``st[0][i]``都为``0``**。后面的详细代码我会演示dfs打表的具体实现。
## 查询操作
给出两个节点``u``和``v``，我们先**将两个节点的高度化为一致**，然后共同向上寻找，直到找到公共祖先为止。

## 代码实现
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5e5+10;
int n,m,s;
vector<int> e[N];//树
int st[N][20];//这个20是根据N的范围来定的，2^20>5e5
int dep[N];//树的高度
void dfs(int u,int father)
{
    dep[u]=dep[father]+1;//u的高度是父节点的高度加1
    st[u][0]=father;//u向上搜索1个节点就是父节点
    for(int i=1;i<=19;i++)
    {
        st[u][i]=st[st[u][i-1]][i-1];//搜索u的所有祖先
    }
    for(auto it=e[u].begin();it!=e[u].end();++it)
    {
        if(*it!=father)dfs(*it,u);//对u的子节点进行同样的操作
    }
}
int lca(int u,int v)
{
    if(dep[u]<dep[v])swap(u,v);//如果u的高度小于v的高度，交换两个节点
    for(int i=19;i>=0;i--)
    {
        if(dep[st[u][i]]>=dep[v])u=st[u][i];//让u与v的高度一致
    }
    if(u==v)return v;//如果v是u的祖先，最近公共祖先就是v
    for(int i=19;i>=0;i--)
    {
        if(st[u][i]!=st[v][i])u=st[u][i],v=st[v][i];//u与v同时向上搜索，为了防止找到的祖先不是最近公共祖先，我们去找最近公共祖先的子节点
    }
    return st[u][0];//返回最近公共祖先
}
int main()
{
    cin>>n>>m>>s;
    for(int i=1;i<n;i++)//建树
    {
        int x,y;
        cin>>x>>y;
        e[x].push_back(y);
        e[y].push_back(x);
    }
    dfs(s,0);//打表
    for(int i=1;i<=m;i++)
    {
        int a,b;
        cin>>a>>b;
        cout<<lca(a,b)<<'\n';
    }
    return 0;
}
```
