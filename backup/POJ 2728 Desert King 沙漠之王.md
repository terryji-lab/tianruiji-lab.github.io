# POJ 2728 Desert King

## 题目描述

大卫大帝刚刚成为一个沙漠国家的国王。为了赢得人民的尊敬，他决定在全国各地修建水渠，将水引到每一个村庄。凡是与他的首都相连的村庄都将得到灌溉。作为国家的统治者与智慧的象征，他希望以最优雅的方式修建这些水渠。

经过多日研究，他终于制定出了计划。他希望水渠每英里的平均成本最小化。换句话说，水渠的总花费与总长度的比值必须最小。他只需要修建必要的水渠将水送到所有村庄，这意味着每个村庄到首都只有一条路径。

他的工程师们勘测了全国，记录了每个村庄的位置和海拔。所有的水渠必须在两个村庄之间笔直修建，并保持水平。由于每两个村庄的海拔都不同，工程师们得出结论：每条水渠需要配备一个垂直的扬水装置，可以将水提升或让水流下。水渠的长度是两个村庄之间的水平距离，水渠的花费是扬水装置的高度。注意：每个村庄的海拔各不相同，且不同的水渠不能共用一个扬水装置。水渠可以安全交叉，且没有三个村庄位于同一直线上。

作为大卫国王的首席科学家和程序员，你需要找出修建水渠的最优方案。

## 输入格式

输入包含多个测试用例。每个测试用例以一个整数 N（2 ≤ N ≤ 1000）开头，表示村庄的数量。接下来 N 行，每行包含三个整数 x, y, z（0 ≤ x, y < 10000，0 ≤ z < 10000000），表示一个村庄的坐标和海拔。

## 输出格式

对于每个测试用例，输出一行一个数字，表示最小的总花费与总长度之比，结果四舍五入保留三位小数。

## 样例输入
```
4
0 0 0
0 1 1
1 1 2
1 0 3
0
```
## 样例输出
```
1.000
```
## 分析
**水渠的总花费与总长度的比值必须最小，水渠的长度是两个村庄之间的水平距离，水渠的花费是扬水装置的高度**
这道题是一个分数规划最小化的问题，分子为总花费即**村庄间高度差之和**，分母为总长度即**村庄间水平距离之和**，要让这个值x最小，也就是以 $\sum \text{cost} - x \cdot \sum \text{len} \le 0$ 为条件进行二分搜索找到最小值。
每个村庄到首都只有一条路径，说明最后所有村庄包括首都之间都是连通的，并且我们要寻找 $\sum \text{cost} - x \cdot \sum \text{len}\$ 的最小值，我们就可以把问题转化一下 —— 用图来存储村庄节点，所有节点两两相连并且以 $\text{cost}_i - x \cdot \text{len}_i$ 为边上权重的**最小生成树问题**，当我们算出来的**最小边权值小于等于0时，说明x是可行值，继续搜索更小值**，使用prim算法。
代码如下
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1010;
const double INF=1e9;
const double eps=1e-9;
int n;
int x[N];
int y[N];
int z[N];
double w[N][N];
double dist[N];
bool used[N];
double get_dh(int i,int j)//计算高度差即cost
{
	return abs(z[i]-z[j]);
}
double get_dist(int i,int j)//计算距离即len
{
	return sqrt((x[i]-x[j])*(x[i]-x[j])+(y[i]-y[j])*(y[i]-y[j]));
} 
void get_w(double v)//计算每两个点的边权，存入w数组
{
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			w[i][j]=get_dh(i,j)-v*get_dist(i,j);
		}
	}
}
bool prim_check(double v)//最想生成树prim算法
{
	get_w(v);
	memset(used,false,sizeof(used));
	double res=0;
	for(int i=1;i<=n;i++)
	{
		dist[i]=w[1][i];
	}
	dist[1]=0;
	used[1]=true;//以首都为生成树的第一个点
	for(int i=1;i<n;i++)//n个点需要n-1条边
	{
		int cur=-1;
		double min_dist=INF;
		for(int j=1;j<=n;j++)
		{
			if(dist[j]<min_dist && used[j]==false)//寻找距离生成树最近的点
			{
				cur=j;
				min_dist=dist[j];
			}
		}
		used[cur]=true;//将点加入生成树
		res+=min_dist;//更新总边权
		for(int j=1;j<=n;j++)//更新每个点距离最小生成树的距离
		{
			if(!used[j])
			{
				dist[j]=min(w[cur][j],dist[j]);
			}
		}
	}
	return res<eps;//如果总权和小于等于0则说明v是可行分数值
}
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>x[i]>>y[i]>>z[i];
	}
	double l=0,r=1e9;
	while(r-l>eps)
	{
		double mid=l+(r-l)/2;
		if(prim_check(mid))r=mid;//如果可行左移右指针寻找更小的分数值
		else l=mid;
	}
	cout<<fixed<<setprecision(3)<<r;
	return 0;
}
```