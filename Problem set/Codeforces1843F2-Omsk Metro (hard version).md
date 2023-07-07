[题目链接](https://codeforces.com/contest/1843/problem/F2)



题目大意：

给出一棵树，树上的每个点的点权为 $1$ 或 $-1$ ，给出若干询问，每个询问 $u, v, x$ 问 $u$ 到 $v$ 的路径上是否存在一个子路径，其上的点权加和等于 $x$ 。

题解：

首先考虑 $u$ 永远为 $1$ 的情况，即本题的 $easy\ version$ ，只要对每个点 $v$ ，求点 $1$ 到 点 $v$ 的最大、最小子串，并验证 $x$ 是否在最大最小点权和区间内即可，由于点权和的变化是连续的，如果 $x$ 在此区间内，一定在某处的点权加和等于 $x$ 。

现在考虑 $u$ 和 $v$ 为任意点的情况，该种情况下，可以在向树中添加每个点的同时，用树上倍增维护当前点到其各个祖先节点的区间前缀、后缀信息（类似于，将当前节点到根节点的路径视作一个区间，然后对这个区间建立 $st$ 表），时间复杂度为 $O(nlogn)$ 。

查询时，找到 $u$ 和 $v$ 的 $LCA$ ，然后将两端区间信息合并即可。

代码：

```c++
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
#define FOR(i, a, b) for(ll i = a; i <= b; i ++ )
#define pb push_back
using namespace std;
using ll = long long;
using pll = pair<ll,ll>;
const ll N = 2e5 + 5;
const ll mod = 1e9 + 7;
const ll lg = 17;
int n;
struct info{
	int sum, min_prel, max_prel, min_prer, max_prer;
	int max_seg;
	int min_seg;
	info(int v = 0)
	{
		sum = v;
		min_prel = min_prer = min_seg = min(v, 0);
		max_prel = max_prer = max_seg = max(v, 0);
	}	
};
info merge(info& a, info& b)
{
	info res;
	res.sum = a.sum + b.sum;
	res.min_prel = min(a.min_prel, a.sum + b.min_prel);
	res.min_prer = min(b.min_prer, a.min_prer + b.sum);
	res.max_prel = max(a.max_prel, a.sum + b.max_prel);
	res.max_prer = max(b.max_prer, a.max_prer + b.sum);
	res.max_seg = max({a.max_seg, b.max_seg, a.max_prer + b.max_prel});
	res.min_seg = min({a.min_seg, b.min_seg, a.min_prer + b.min_prel});
	return res;
}
int up[lg + 1][N];//ancestor
info ans[lg + 1][N];
int d[N];//depth
void solve()
{
	cin >> n;
	FOR(i, 0, lg) up[i][0] = 0;
	ans[0][0] = info(1);
	d[0] = 0;
	int cur = 0;
	FOR(i, 1, n)
	{
		char ch;
		cin >> ch;
		
		if(ch == '+')
		{
			int v, x;
			cin >> v >> x;
			
			v -- ;
			cur ++ ;
			
			d[cur] = d[v] + 1;
			up[0][cur] = v;
			
			FOR(j, 0, lg - 1) up[j + 1][cur] = up[j][up[j][cur]];
			
			ans[0][cur] = info(x);
			FOR(j, 0, lg - 1) ans[j + 1][cur] = merge(ans[j][cur], ans[j][up[j][cur]]);
		}
		else
		{
			int u, v, x;
			cin >> u >> v >> x;
			u -- ;
			v -- ;
			if(d[u] < d[v])
			{
				swap(u, v);
			}
			
			int dif = d[u] - d[v];
			info a, b;
			for(int j = lg; j >= 0; j -- )
			{
				if((dif >> j) & 1)
				{
					a = merge(a, ans[j][u]);
					u = up[j][u];
				}
			}
			
			if(u == v)
			{
				a = merge(a, ans[0][u]);
			}
			else
			{
				for(int j = lg; j >= 0; j -- )
				{
					if(up[j][u] != up[j][v])
					{
						a = merge(a, ans[j][u]);
						u = up[j][u];
						b = merge(b, ans[j][v]);
						v = up[j][v];
					}
				}
				a = merge(a, ans[1][u]);
				b = merge(b, ans[0][v]);
				
			}
			swap(b.min_prel, b.min_prer);
			swap(b.max_prel, b.max_prer);
			info res = merge(a, b);
			if(x >= res.min_seg && x <= res.max_seg)
			{
				cout << "YES\n";
			}
			else
			{
				cout << "NO\n";
			}
		}
	}
}
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```

