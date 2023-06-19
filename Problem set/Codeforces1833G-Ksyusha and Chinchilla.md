[题目链接](https://codeforces.com/contest/1833/problem/G)



题目大意：

给出一颗树，将其切分为每三个节点一组的子树。



题解：

考虑一个节点，假设其有 $n$ 个叶子节点：

若 $n >= 3$ ，无解；

若 $n = 2$ ，将其和叶子节点切分出去；

若 $n = 1$ ，将其叶子节点连接到当前节点的父节点，可以转换为 $n = 2$ 的情况；

注意处理非叶子节点大于 $1$ 的情况。



代码：

```c++
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
#define _for(i, a, b) for(ll i = a; i <= b; i ++ )
using namespace std;
using ll = long long;
using pll = pair<ll,ll>;
const ll N = 2e5 + 5;
const ll mod = 1e9 + 7;
ll n;
vector<ll> g[N];
ll vis[N];
/*
1
9
2 4
3 4
4 5
5 6
9 4
9 8
1 3
7 1
*/
/*
1
15
1 2
1 3
1 4
2 5
2 6
4 7
6 9
7 12
12 13
9 8
9 11
9 10
10 14
14 15
*/
void solve()
{
	cin >> n;
	map<pll, ll> mp;
	for(ll i = 1; i <= n; i ++ )
	{
		g[i].clear();
		vis[i] = 0;
	}
	for(ll i = 1; i <= n - 1; i ++ )
	{
		ll u, v;
		cin >> u >> v;
		g[u].push_back(v);
		g[v].push_back(u);
		mp[{u, v}] = mp[{v, u}] = i;
	}
	
	if(n % 3 != 0)
	{
		cout << -1 << endl;
		return;
	}
	
	queue<ll> q;
	for(ll i = 1; i <= n; i ++ )
	{
		if(g[i].size() == 1)
		{
			q.push(i);
		}
	}
	vector<pll> ans;
	/*
6
2 3
3 5
3 6
1 6
2 4
	*/
	while(!q.empty())
	{
		ll top = q.front();

		q.pop();

		if(vis[top] == 1)
		{
			continue;
		}
		ll son = 0;
		for(auto i : g[top])
		{
			if(g[i].size() == 1)
			{
				son ++ ;
			}
		}
		if(son >= 3)
		{
			cout << -1 << endl;
			return;
		}
		else if(son == 2)
		{
			ll cnt = 0;
			for(auto i : g[top])
			{
				if(!vis[i])
				{
					//debug(i);
					if(g[i].size() == 1)
					{
						vis[i] = 1;
						cnt ++ ;
						if(cnt > 2)
						{
							cout << -1 << endl;
							return;
						}
						continue;
					}
					q.push(i);
					ans.push_back({top, i});
					for(vector<ll>::iterator j = g[i].begin(); j != g[i].end(); j ++ )
					{
						if(*j == top)
						{
							g[i].erase(j);
							break;
						}
					}
	
				}
			}
			vis[top] = 1;
		}
		else if(son == 1)
		{
			if(g[top].size() - son > 1)
			{
				continue;
			}
			int cnt = 0;
			for(auto i : g[top])
			{
				if(!vis[i] && g[i].size() > 1)
				{
					
					q.push(i);
					for(vector<ll>::iterator j = g[top].begin(); j != g[top].end(); j ++ )
					{
						if(*j != i)
						{
							g[i].push_back(*j);
							g[top].erase(j);
							break;
						}
					}

				}
			}

		}
		else if(son == 0)//leaf
		{
			for(auto i : g[top])
			{
				if(!vis[i])
				{
					
					if(g[i].size() > 1)
					{
						q.push(i);
					}
					//deg[i] -- ;
				}
			}
		}
		
	}

	for(ll i = 1; i <= n; i ++ )
	{
		if(!vis[i])
		{

			cout << -1 << endl;
			return;
		}
	}
	
	cout << ans.size() << endl;
	for(auto i : ans)
	{
		cout << mp[{i.first, i.second}] << ' ';
	}
	cout << endl;
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

