[题目链接](https://atcoder.jp/contests/abc252/tasks/abc252_e)

题目大意：

给出一张无向图，求令节点$1$到其他节点道路距离和最小的一棵树。

题解：

跑一遍$dijkstra$，生成一棵树即可；由于需要输出选择了哪些边，我们可以记录到每个节点最后是从哪条边最后访问的，在当前节点从堆中弹出时将这条边加入答案即可。

代码：
```cpp
#include<iostream>
#include<algorithm>
#include<set>
#include<cstring>
#include<vector>
#include<queue>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
const ll N = 2e5 + 5;
const ll M = 100 + 5;
ll n,m;
struct node{
	ll v,w,id;
};
vector<node>g[N];
vector<ll>ans;
ll dis[N];
ll st[N];
ll pre[N];
struct cmp{
	bool operator()(const node a,const node b)const{
		return dis[a.id] > dis[b.id];
	}
};
void dijkstra()
{
	memset(dis,0x3f,sizeof dis);
	priority_queue<pll,vector<pll>,greater<pll> >heap;
	dis[1] = 0;
	heap.push({0,1});
	while(!heap.empty())
	{
		auto t = heap.top();
		heap.pop();
		ll d = t.first;
		ll ver = t.second;
		if(st[ver])continue;
		st[ver] = 1; 
		if(pre[ver] != 0)ans.pb(pre[ver]);
		for(auto i : g[ver])
		{
			if(dis[ver] + i.w < dis[i.v])
			{
				dis[i.v] = dis[ver] + i.w;
				pre[i.v] = i.id;
				heap.push({dis[i.v],i.v});
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
		ll u,v,w;
		cin >> u >> v >> w;
		g[u].pb({v,w,i});
		g[v].pb({u,w,i});
	}
	dijkstra();
	for(auto i : ans)cout << i << ' ' ;
	
}
```