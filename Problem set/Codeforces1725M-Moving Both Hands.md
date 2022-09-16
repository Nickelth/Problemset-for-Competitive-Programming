[题目链接](https://codeforces.com/contest/1725/problem/M)

题目大意：

在一张有向带权图上有两个点，每次可以将一个点移到另一个点，花费就是边权，求两个点相遇的最小花费.

第一个点总在节点 $1$ ，对第二个点在 $2$ , $3$ ,……, $n$ 的情况分别求答案.

题解：

建图上很有意思的一道题.

有两种可能的情况，一是从点 $1$ 直接走到目标点 $t$ ，另一种是点 $1$ 走到中继点 $k$ ，另一个点从 $t$ 走到 $k$ ，在这里我们将第一种情况视为平凡情况.

那么，答案就分为两个阶段：从点 $1$ 走到 $k$ ，然后从 $k$ 沿反图的边走到 $t$ ，两个阶段分别求最短路.

或者说，求从点 $1$ 走到 $t$ 的最短路，但是在中间某个节点我们将整张图变为反图，这个变换在路径中只进行一次.

那么，如何表达出“在某个节点进行且仅进行一次将图变为反图”的操作呢？

我们可以用特殊的建图方式来做到这一点.

首先，拆点，将每个点 $x$ 拆成两个域： $(x,0)$ 和 $(x,1)$ 分别表示 $x$ 在原图和反图上的对应点.

然后，对原图的每条边 $(u,v)$ ，转换为从 $(u,0)$ 到 $(v,0)$ 连一条边，再从 $(v,1)$ 到 $(u,1)$ 连一条边.

最后，对每个点 $x$ ，连一条从 $(x,0)$ 到 $(x,1)$ 的边，权重为 $0$ .

在这张图上求单源最短路即可，比如 $1$ 到 $x$ 的答案就是从 $(1,0)$ 到 $(x,1)$的最短路.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define debug(x) cout << #x << " = " << (x) << endl
#define pll pair<ll,ll>
#define INF 2e15 + 200
using namespace std;
const ll mod = 1e9 + 7;
const ll N = 2e5 + 5;
ll n, m;
struct edge{
	ll to,flip,w;
	edge(ll b, ll c, ll d)
	{
		to = b;
		flip = c;
		w = d;
	}
};
struct func{
	bool operator()(edge a, edge b)
	{
		return a.w > b.w;
	}
};
vector<edge> g[N][2];
ll dis[N][2];
int st[N][2];
void dijkstra()
{
	priority_queue<edge, vector<edge>, func >heap;
	
	for(ll i = 1; i <= n; i ++ )
	{
		dis[i][0] = dis[i][1] = INF;
	}
	
	dis[1][0] = 0;
	edge e(1ll,0ll,0ll);
	heap.push(e);
	
	while(!heap.empty())
	{
		auto t = heap.top();
		heap.pop();
		
		if(st[t.to][t.flip])continue;
		
		st[t.to][t.flip] = 1;
		
		for(auto i : g[t.to][t.flip])
		{
			if(dis[i.to][i.flip] > dis[t.to][t.flip] + i.w)
			{
				dis[i.to][i.flip] = dis[t.to][t.flip] + i.w;
				
				i.w = dis[i.to][i.flip];
				
				heap.push(i);
			}
		}
	}
}
int main()
{
	IOS;
	
	cin >> n >> m;
	
	for(ll i = 1; i <= m; i ++ )
	{
		ll u, v, w;
		cin >> u >> v >> w;
		edge e(v,0,w);
		g[u][0].push_back(e);
		e.flip = 1;
		e.to = u;
		g[v][1].push_back(e);
	}
	
	for(ll i = 1; i <= n; i ++ )
	{
		edge e(i,1,0);
		g[i][0].push_back(e);
	}
	
	dijkstra();
	
	for(ll i = 2; i <= n; i ++ )
	{
		if(dis[i][1] >= INF)
		{
			cout << -1 << ' ';
		}
		else
		{
			cout << dis[i][1] << ' ';
		}
	}
}
```



