# Bellman-Ford 算法
## 算法简介
Bellman-Ford 算法是一种用于求解**单源最短路径**的算法，和Dijkstra算法相比，它最大的特点是**可以处理带有负权边的图**，并且能够**检测图中是否存在负权环**，但是相应的它的时间复杂度相对较高。

## 核心思想

Bellman-Ford 算法基于**动态规划**和**松弛操作**。

### 松弛操作

对于一条边 `(u, v)`，权值为 `w`，如果当前从源点 `s` 到 `u` 的距离 `dist[u]` 已知，并且 `dist[u] + w < dist[v]`，那么就更新 `dist[v] = dist[u] + w`。这个更新过程称为“松弛”。''松弛''n次就代表目前`dist[]`中**存放的是最多经历n条边得到的最短路径**。

### 核心原理

在一个含有 `n` 个顶点的图中，任意一条最短路径（无负环）最多包含 `n-1` 条边。  
因此，我们对所有边进行 **n-1 轮** 松弛，每一轮松弛都尝试将最短路径的长度扩展一条边。经过 n-1 轮后，`dist[]` 中存储的就是从源点出发的最短距离。

## 算法步骤

1. **初始化**：将源点 `s` 的距离设为 0，其他所有顶点的距离设为无穷大（`inf`）。
2. **松弛所有边**：重复执行以下操作 `n-1` 次  
   - 遍历图中的每一条边 `(u, v, w)`  
   - 如果 `dist[u] != inf` 且 `dist[u] + w < dist[v]`，则更新 `dist[v] = dist[u] + w`
3. **检测负权环**：再对所有边进行第 `n` 轮松弛  
   - 如果还有任何边能够被松弛，说明图中存在**从源点可达的负权环**，最短路径无解（距离可无限减小）。
4. **输出结果**：若无负环，`dist[]` 即为最短距离；若有负环，一般代表最短路无解

给出代码实现如下：
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=100;
const int inf=0x3f3f3f3f;//极大值
int n,m,s;
struct edge{int v,w;};//边指向的点和边的权值
vector<edge> e[N];//使用邻接表来实现图的存储
int d[N];//最小距离数组
bool bellmanford(int s)
{
	memset(d,inf,sizeof(d));//设置初始距离为无穷远
	d[s]=0;//起点距离为0
	bool flag=false;//是否进行松弛操作
	for(int i=1;i<=n;i++)//进行n轮松弛
	{
		flag=false;
		for(int u=1;u<=n;u++)//对每一个节点的邻边进行松弛操作
		{
			if(d[u]==inf)continue;//如果这个点还没有被连通无需进行松弛操作
			for(auto it=e[u].begin();it!=e[u].end();++it)
			{
				int v=it->v,w=it->w;
				if(d[v]>d[u]+w)//松弛操作
				{
					d[v]=d[u]+w;
					flag=true;
				}
			}
		}
		if(!flag)break;//没有进行松弛说明已经全部到达最短路径，退出循环，如果没有负权环的话n-1轮就会退出，flag为false
	}
	return flag;//找到所有点的最短路径最多需要n-1轮，如果第n轮还进行了松弛操作说明有负权环，最短路径不存在，flag为true
}
int main()
{
	cin>>n>>m>>s;//节点数，边数，起点
	for(int i=1;i<=m;i++)
	{
		int a,b,c;
		cin>>a>>b>>c;//从a到b权重为c的边（有向图）
		e[a].push_back({b,c});//建立邻接表
	}
        return 0;
}
```