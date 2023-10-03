[题目链接](https://codeforces.com/problemset/problem/1851/F)

题目大意：

给出一个数列 $\{a\}$ 和整数 $k$ ，求整数 $x < 2^k$ 使得 $(a_i\bigoplus x)\&(a_j\bigoplus x)$ 最大，其中 $a_i$ 和 $a_j$ 是 $\{a\}$ 中任意两个元素。

题解：

解法1：

dfs

将问题转化为：从 $\{a\}$ 中求两个元素 $a_i$ 和 $a_j$ 令 $a_i \& a_j$ 最大，求出二者后可以构造出x

用dfs，枚举每一位，每次将当前位相同的数分为一组，然后递归搜索

注意需要处理这种情况：两个数前面都相同，在某一位不同，后面又相同，如果这种数对比较多，需要比较其所得最终答案哪个大

应该还有一些细节要注意，但我记不清了

复杂度 O(nk)

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
ll n, k;
ll a[N];
ll final_len = -1;
ll ans = -1;
ll id1, id2;
ll fill_ans(ll arg, ll id1, ll id2)
{
	for(ll i = k; i >= 0; i -- )
	{
		ll cur = (1ll << i);
		ll new_ans = arg ^ cur;
		if((a[id1] & cur) == 0 && (a[id2] & cur) == 0 && (arg & cur) == 0)
		{
			arg ^= cur;
		}

	}
	return arg;
}
ll cal(ll ans, ll id1, ll id2)
{
	return ((ans ^ a[id1]) & (ans ^ a[id2]));
}
void dfs(ll cur, ll len, vector<ll> arr, ll cur_res)
{
	if(len > final_len || cal(ans, id1, id2) < cal(fill_ans(cur_res, arr[0], arr[1]), arr[0], arr[1]))
	{
		final_len = len;
		id1 = arr[0];
		id2 = arr[1];
		ans = fill_ans(cur_res, id1, id2);
		//ans = cur_res;
	}
	if(cur < 0)
	{
		return;
	}
	//debug(arr.size());
	vector<ll> arr1, arr2;
	for(auto i : arr)
	{
		ll num = a[i];
		if((num & (1ll << cur)) != 0)
		{
			arr1.push_back(i);
		}
		else
		{
			arr2.push_back(i);
		}
	}
	//debug(arr1.size());
	//debug(arr2.size());
	if(arr1.size() > 1)
	{
		dfs(cur - 1, len + 1, arr1, cur_res);
	}
	if(arr2.size() > 1)
	{
		dfs(cur - 1, len + 1, arr2, cur_res ^ (1ll << cur));
	}
}
void solve()
{
	cin >> n >> k;
	k -- ;
	vector<ll> arr;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
		arr.push_back(i);
	}
	final_len = -1;
	ans = -1;
	id1 = -1, id2 = -1;
	dfs(k, 0, arr, 0);
	ans = fill_ans(ans, id1, id2);
	cout << id1 << ' ' << id2 << ' ' << ans << endl;
	//cout << "got res : " << ((ans ^ a[id1]) & (ans ^ a[id2])) << endl;
}
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```

 解法2：

trie树，核心逻辑与上一种解法相同

记得空间开大

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define debug(x) cout << #x << " = " << (x) << endl
#define pll pair<ll,ll>
using namespace std;
const ll N = 1e7 + 5;
ll n, k;
ll a[N];
ll node[N][2];
ll tend[N];
ll cnt = 0;
ll cal(ll i, ll j, ll x)
{
	return ((a[i] ^ x) & (a[j] ^ x));
}
pll find(ll cur)
{
	ll iter = 0;
	ll x = 0;
	for(ll i = k; i >= 0; i -- )
	{
		ll b = ((cur & (1 << i)) != 0);
		//debug(b);
		//debug(node[iter][b]);
		//debug(iter);
		if(node[iter][b] != 0)
		{
			//cout << "choose b" << endl;
			iter = node[iter][b];
			if(b == 0)
			{
				x |= (1 << i);
			}
		}
		else if(node[iter][1 - b] != 0)
		{
			//cout << "choose 1-b" << endl;
			iter = node[iter][1 - b];
		}
	}
	return {tend[iter], x};
}
void add(ll cur, ll i)
{
	ll iter = 0;
	for(ll i = k; i >= 0; i -- )
	{
		ll b = ((cur & (1 << i)) != 0);
		if(node[iter][b] != 0)
		{
			iter = node[iter][b];
		}
		else
		{
			node[iter][b] = ++cnt;
			iter = cnt;
		}
	}
	tend[iter] = i;
}
void solve()
{
	cin >> n >> k;
	k -- ;
	for(ll i = 0; i <= cnt; i ++ )
	{
		node[i][0] = node[i][1] = 0;
		tend[i] = 0;
	}
	cnt = 0;
	ll ansi = 0, ansj = 0, ansx = 0, ans = -1;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
		if(i == 1) {
			add(a[i], i);
			continue;
		}
		//debug(i);
		pll p = find(a[i]);
		ll j = p.first;
		ll x = p.second;
		
		//debug(j);
		//debug(x);
		if(cal(i, j, x) > ans)
		{
			ansi = i;
			ansj = j;
			ans = cal(i, j, x);
			ansx = x;
		}
		add(a[i], i);
		
	}
	cout << ansi << ' ' << ansj << ' ' << ansx << endl;
	//debug(cal(ansi, ansj, ansx));
	
}
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```

解法3：

将 $\{a\}$ 升序排序，答案一定在相邻的元素中取。

证明：

取 $1 <= i < j <= n$  , $j - i > 1$ , $a_i$ 和 $a_j$ 如果不相同，那么二者一定有一个相同前缀，该前缀后的位 $t$ ， $a_i$ 的第 $t$ 位为0，而 $a_j$ 的第 $t$ 位为1，而此时 $a_{i + 1}$ 的第 $t$ 位要么为0， 要么为1， 前一种情况下，选择 $a_i$ 和 $a_{i + 1}$ 为答案，是更好的选择；后一种情况，选择 $a_{i + 1}, a_j$ 为答案是更好的选择。

代码：略