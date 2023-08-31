[题目链接](https://codeforces.com/contest/1856/problem/E1)

题目大意：

给出一棵节点数为n的树，请给树分配1-n的序号，使得满足如下要求的节点对(u,v)最多：

u < anc < v，其中anc是u和v的最近公共祖先。

题解：

树形dp，对于每个节点，对其子树分配序号的最优方式是，令一部分子节点全部小于其序号，另一部分全部大于其序号，其答案等于每个孩子的答案，加上小于其序号的节点数乘以大于其序号的节点数。令这个乘积最大的分配方式是令两种类型的节点数尽量各占一半，用背包dp可解。

代码：

```c++
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
#define pb push_back
using namespace std;
using ll = long long;
using pll = pair<ll,ll>;
const ll N = 5e3 + 5;
vector<ll> g[N];
ll n;
ll amt[N];
ll son[N];
ll dp[N];
ll path[N][N];
ll cal(ll cur)
{
	ll res = 1;
	for(auto i : g[cur])
	{
		res += cal(i);
	}
	amt[cur] = res;
	son[cur] = g[cur].size();

	return res;
}
vector<ll> get_pt(vector<ll> e)
{
	ll mm = 0;
	for(auto i : e)
	{
		mm += amt[i];
	}

	
	for(ll i = 0; i <= e.size(); i ++ )
	{
		for(ll j = 0; j <= mm; j ++ )
		{
			dp[j] = 0;
			path[i][j] = 0;
		}
	}
	vector<ll> res;
	for(ll i = 1; i <= e.size(); i ++ )
	{
		ll cur = e[i - 1];
		ll w = amt[cur];

		for(ll j = mm; j - 2 * w >= 0; j -- )
		{
			
			if(dp[j - 2 * w] + 2 * w > dp[j])
			{
				dp[j] = dp[j - 2 * w] + 2 * w;
				path[i][j] = 1;
			}
		}
	}
	ll i = e.size(), j = mm;
	while(i && j)
	{
		if(path[i][j])
		{
			res.pb(e[i - 1]);
			j -= 2 * amt[e[i - 1]];
		}
		i -- ;
	}
	return res;
}
ll dfs(ll cur)
{

	
	if(son[cur] == 0)
	{
		return 0;
	}
	else if(son[cur] == 1)
	{
		return dfs(g[cur][0]);
	}
	else
	{

		vector<ll> pt = get_pt(g[cur]);
		ll res = 0;
		ll p1 = 0, p2 = 0;
		for(auto i : g[cur])
		{
			res += dfs(i);
		}

		for(auto i : pt)
		{
			p1 += amt[i];

		}

		res += p1 * (amt[cur] - p1 - 1);
		

		return res;
	}
}
void solve()
{
	cin >> n;
	for(ll i = 2; i <= n; i ++ )
	{
		ll cur;
		cin >> cur;
		g[cur].pb(i);
	}
	cal(1);
	cout << dfs(1) << endl;
}
int main()
{
	solve();
}
```

