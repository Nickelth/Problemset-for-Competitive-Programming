[题目链接](https://codeforces.com/contest/1818/problem/D)



题目大意：

给出一个图，寻找图中任意一个满足以下条件的环：

环上存在一个点，与两个不是环上的点相邻。



题解：

若一个环上存在一个度大于 $4$ 的点，一定可以构造出满足条件的环。



代码：

```c++
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
using ll = long long;
using pll = pair<ll,ll>;
const ll N = 2e3 + 5;
ll n, m;
vector<ll> g[N];
vector<pll> ans;

int in(ll cur, vector<ll>& arr)
{
	//cout << "checking " << cur << " " << in_arr[cur] << endl;
	for(ll i = 0; i < arr.size(); i ++ )
	{
		if(arr[i] == cur)
		{
			return 1;
		}
	}
	return 0;
}

int check(vector<ll>& arr, ll st)
{
	

	/*cout << " arr is " << endl;
	for(ll i = st; i < arr.size(); i ++ )
	{
		cout << arr[i] << ' ';
	}
	cout << endl;*/
	if(arr.size() - st < 3)
	{
		return 0;
	}
	for(ll i = st; i < arr.size(); i ++ )
	{
		if(g[arr[i]].size() >= 4)
		{
			return i;
		}
	}
	return -1;
}
void insert(vector<ll>& arr, ll st, ll end, ll flag)
{
	ll st_pos = 0;

	for(ll i = 0; i < arr.size(); i ++ )
	{
		if(arr[i] == st)
		{
			st_pos = i;
		}

	}
	//debug(st_pos);
	int f = 0;
	for(ll i = st_pos; ; )
	{
		ll ne = (i + 1) % (ll)arr.size();

		i = ne;
		if(arr[ne] == flag)
		{
			f = 1;
			break;
		}
		if(arr[ne] == end)
		{
			break;
		}
	}
	
	/*debug(f);
	debug(arr.size());

	debug(st_pos);*/
	for(ll i = st_pos; ;)
	{
		ll ne = -1;
		if(!f)
		{
			ne = (i + 1) % (ll)arr.size();
		}
		else
		{
			ne = i - 1;
			if(ne < 0)
			{
				ne = arr.size() - 1;
			}
		}
 
		ans.push_back({arr[i], arr[ne]});
		i = ne;
		if(arr[ne] == end)
		{
			break;
		}
	}
	
	
}
/*
1
10 11
1 2
2 3
3 4
5 6
6 7
7 8
8 9
9 10
10 5
5 8
5 9
*/
/*
1
7 6
1 4
4 6
2 4
4 7
2 5
5 7
*/
void get_ans(vector<ll>& arr, ll id)
{
	ll n1 = -1, n2 = -1;
	for(ll j = 0; j < g[arr[id]].size(); j ++ )
	{
		if(!in(g[arr[id]][j], arr))
		{
			if(n1 == -1)
			{
				n1 = g[arr[id]][j];
			}
			else
			{
				n2 = g[arr[id]][j];
				break;
			}
		}
	}
	ll c1 = -1, c2 = -1;
	if(id > 0)
	{
		c1 = arr[id - 1];
	}
	else
	{
		c1 = arr[arr.size() - 1];
	}
	if(id < arr.size() - 1)
	{
		c2 = arr[id + 1];
	}
	else
	{
		c2 = arr[0];
	}
	/*debug(n1);
	debug(n2);
	debug(c1);
	debug(c2);*/
	if(n2 != -1)
	{
		
		insert(arr, c1, c2, arr[id]);
		ans.push_back({arr[id], c1});
		ans.push_back({arr[id], c2});
		ans.push_back({arr[id], n1});
		ans.push_back({arr[id], n2});
	}
	else if(n1 != -1 && n2 == -1)
	{
		ans.push_back({n1, arr[id]});
		ans.push_back({arr[id], c1});
		for(ll j = 0; j < g[arr[id]].size(); j ++ )
		{
			if(g[arr[id]][j] != n1 && g[arr[id]][j] != c1 && g[arr[id]][j] != c2)
			{
				ans.push_back({arr[id], g[arr[id]][j]});
				ans.push_back({arr[id], c2});
				insert(arr, g[arr[id]][j], c2, arr[id]);
				break;
			}
		}
	}
	else if(n1 == -1)
	{
		ans.push_back({c2, arr[id]});
		ans.push_back({arr[id], c1});
		for(ll j = 0; j < g[arr[id]].size(); j ++ )
		{
			if(g[arr[id]][j] != c1 && g[arr[id]][j] != c2)
			{
				if(n1 == -1)
				{
					n1 = g[arr[id]][j];
				}
				else if(n2 == -1)
				{
					n2 = g[arr[id]][j];

					ans.push_back({arr[id], n1});
					ans.push_back({arr[id], n2});
					insert(arr, n1, n2, arr[id]);
					break;
				}
			}
		}
	}
}
void dfs(ll cur, vector<ll>& arr, vector<ll>& vis)
{
	if(!ans.empty())
	{
		return;
	}
	

	for(ll i = 0; i < g[cur].size(); i ++ )
	{

		if(!vis[g[cur][i]])
		{
			arr.push_back(g[cur][i]);
			vis[g[cur][i]] = 1;
			
			dfs(g[cur][i], arr, vis);
			if(!ans.empty())
			{
				return;
			}
			
			arr.pop_back();

		}
		else
		{
			if(g[cur][i] == arr[0] && arr.size() > 2)
			{
				/*debug(g[cur][i]);

				debug(arr[0]);
				cout << " arr is " << endl;
				for(ll i = 0; i < arr.size(); i ++ )
				{
					cout << arr[i] << ' ';
				}
				cout << endl;*/
				get_ans(arr, 0);
				return;
			}
			
		

		}

	}
	return;
}

void solve()
{
	cin >> n >> m;
	ans.clear();
	for(ll i = 1; i <= n; i ++ )
	{
		g[i].clear();
	}
	for(ll i = 0; i < m; i ++ )
	{
		ll u, v;
		cin >> u >> v;
		g[u].push_back(v);
		g[v].push_back(u);

	}
	for(ll i = 1; i <= n; i ++ )
	{
		if(g[i].size() >= 4)
		{
			vector<ll> arr;
			vector<ll> vis(n + 1, 0);
			vis[i] = 1; 
			arr.push_back(i);
			dfs(i, arr, vis);
			if(!ans.empty())
			{
				cout << "YES" << endl;
				cout << ans.size() << endl;
				for(auto j : ans)
				{
					cout << j.first << ' ' << j.second << endl;
				}
				return;
			}
		}
	}
	

	cout << "NO" << endl;

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

