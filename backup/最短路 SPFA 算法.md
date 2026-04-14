# 算法简介
SPFA，又叫做最短路快速算法，其实就是BellmanFord算法的队列优化版本，在时间复杂度更低的同时依旧可以对于负权环进行判断。
## 核心思想
只有在**节点距离出发点的距离变化了的情况下才会需要对这个节点的相邻点进行更新**。
我们使用**队列**，如果一个点距离出发点**距离变化并且这个点不在队列中**，那么让它入队。这个点出队时，就对其相邻的点的最小距离进行更新，也就是进行**松弛操作**，然后把**距离发生了变化的点加入队列，直到队列为空**。我们使用数组``in_que[]``来记录一个点是否已经入队，来防止一个点重复入队。是不是非常的像**广度优先搜索**？
同时我们也可以设置一个``pre[]``数组来记录路径，``cnt[]``数组来记录每个节点在距离出发点最近时通过的边数，判断**是否有负权环**。因为一条最短路径在有``n``个节点并且**没有负权环的情况下，最多通过``n-1``条边**。
## 算法步骤

1. 初始化：将源点``s``的距离设为0，其余点的距离设为无穷大，让源点入队。
2. 弹出队列首个元素，对其相邻节点进行松弛操作，**如果改变了某个节点的距离，增加这个点的``cnt``记数和``pre``前驱点**，并且这个点**不在队列中，就让这个节点入队**。
3. 如果这个点的``cnt``大于等于``n``，说明**存在负权边，最短路径无解**，结束循环。
4. 否则在**队列为空**之前重复以上操作。
5. 可以通过``pre[]``来输出到达每一个点的最短路径

给出代码实现如下
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=100;
const int inf=0x3f3f3f3f;
struct edge{int v,w;};
queue<int> q;
vector<edge> e[N];
int n,m,s;
int dist[N];
int pre[N];
int cnt[N];
bool in_que[N];
bool SPFA(int s)
{
	memset(dist,inf,sizeof(dist));
	q.push(s);
	dist[s]=0,pre[s]=s,cnt[s]=0,in_que[s]=true;//将源点入队，初始化
	while(!q.empty())//当队列不为空循环
	{
		int cur=q.front();
		q.pop();//弹出队首元素
		in_que[cur]=false;//标记出队
		for(auto it=e[cur].begin();it!=e[cur].end();++it)
		{
			int v=it->v,w=it->w;
			if(dist[v]>dist[cur]+w)//对相邻节点进行松弛操作
			{
				dist[v]=dist[cur]+w;
				cnt[v]=cnt[cur]+1;
				pre[v]=cur;//更新dist，cnt，pre
				if(cnt[v]>=n)return true;//如果经过边数大于n-1，存在负权环，返回true
				if(!in_que[v])q.push(v),in_que[v]=true;//如果不在队列中就入队
			}
		}
	}
	return false;//不存在负权环，返回false
}
void dfs_path(int v)//递归打印路径
{
	if(v==pre[v])//如果是源点，递归结束
	{
		cout<<v<<' ';
		return;
	}
	dfs_path(pre[v]);//先打印前驱点
	cout<<v<<' ';
}
int main()
{
	cin>>n>>m>>s;
	for(int i=1;i<=m;i++)//创建邻接表
	{
		int a,b,c;
		cin>>a>>b>>c;
		e[a].push_back({b,c});
	}
	return 0;
}
```