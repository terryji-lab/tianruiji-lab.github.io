# Kruskal算法
利用并查集求最小生成树。与Prim算法以点为单位不同，**Kruskal算法以边为单位，每次贪心选取最短的边来构造生成树，利用并查集保证选取的边不会构成环**。
## 算法步骤

1. 初始化并查集，把``n``个点放到``n``个独立的集合
2. 将所有的边按照边权**小到大**排序。（贪心思想）
3. 按顺序枚举每一条边，如果这条边连接的两个点**不属于同一个集合，就把这条边加入最小生成树，然后合并这两个集合**。如果这两个点已经在同一个集合，就直接跳过。
4. 重复执行步骤``3``，``n``个点总共选取``n-1``条边。

## 代码
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=2e5+5;
int n,m,ans,cnt;
struct edge
{
    int u,v,w;//连接的点u和v，还有权重w
    bool operator<(const edge&t)const//重载运算符用于排序
    {
        return w<t.w;//升序
    }
}e[N];
//并查集
int fa[N];
int find(int u)
{
    if(fa[u]==u)return u;
    return fa[u]=find(fa[u]);
}
void unionset(int u,int v)
{
    fa[find(u)]=find(v);
}
bool kruskal()
{
    sort(&e[1],&e[1]+m);//排序
    for(int i=1;i<=n;i++)fa[i]=i;//初始化并查集
    for(int i=1;i<=m;i++)//遍历所有的边
    {
        int fu=find(e[i].u);
        int fv=find(e[i].v);
        if(fu!=fv)//如果u与v不在一个集合中
        {
            unionset(e[i].u,e[i].v);//合并u与v的集合
            cnt+=1;//边数加一
            ans+=e[i].w;//权重增加
        }
    }
    return cnt==n-1;//是否有n-1条边，判断是否连通
}
int main()
{
    cin>>n>>m;
    for(int i=1;i<=m;i++)
    {
        cin>>e[i].u>>e[i].v>>e[i].w;
    }
    if(kruskal())cout<<ans;
    else cout<<"orz";
    return 0;
}

```