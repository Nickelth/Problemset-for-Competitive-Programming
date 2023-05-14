[题目链接](https://codeforces.com/contest/1802/problem/E)



题目大意：

有 $n$ 个数组，问如何排列数组的顺序，使按序遍历所有数组所得的价值最大。

按序遍历所有数组的价值如此计算：从第一个数开始，每遇到一个比之前所有数大的数，则价值加一。



题解：

首先注意到，对每个数组，可以将比之前数小的数删去，只保留以第一个数为起点的严格升序序列，不会对答案有影响。

其次，注意到若将一个最大值为 $M_i$ 的数组排到最大值为 $M_j$ 的数组后，且 $M_j > M_i$ 时，最大值为 $M_i$ 的数组贡献的价值为零。

令 $dp_i$ 表示以第 $i$ 个数组结尾所得到的最大价值，将所有数组按最大值升序排列，考虑若加入一个新的数组（最大值比之前所有数组的大），当该数组插入第 $j$ 个数组之后时，所得的价值是 $dp_i = dp_j + val$ ，其中 $val$ 是 $dp_i$ 中比第 $j$ 个数组最大值大的那些数字所贡献的价值。

注意到数据范围，所有数字加起来不超过 $2e5$ ，所以对每个数组，遍历其中的数字，对每个数字 $m$ ，通过二分查找在之前的数组中最大值比它小、且最大值最大的数组，计算将当前数组插入该数组后的价值，维护价值最大值即 $dp_i$ 即可。

由于 $dp_i$ 是非降的，答案是 $dp_n$ 。

注意直接对数组排序会超时，应改为对索引排序。

代码：

```c++
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
using ll = long long;
ll n;
void solve()
{
	cin >> n;
	vector<vector<ll> > arr(n);
	vector<ll> id(n);
	for(ll i = 0; i < n; i ++ )
	{
		ll k;
		cin >> k;
		ll mx = -1;
		for(ll j = 0; j < k; j ++ )
		{
			ll a;
			cin >> a;
			if(a > mx)
			{
				mx = a;
				arr[i].emplace_back(a);
			}
		}
		id[i] = i;
	}

	sort(id.begin(), id.end(), [&](ll t1, ll t2){
		return arr[t1].back() < arr[t2].back();
	});
	vector<ll> dp(n, 0);

	for(ll i = 0; i < n; i ++ )
	{
		ll cur = id[i];
		if(i == 0)
		{
			dp[i] = arr[cur].size();
			//debug(i);
			//debug(dp[i]);
			continue;
		}
		dp[i] = max(dp[i - 1], (ll)arr[cur].size());
		for(ll j = 0; j < arr[cur].size(); j ++ )
		{
			ll l = 0, r = i - 1;
			while(l < r)
			{
				ll mid = (l + r + 1) / 2;
				if(arr[id[mid]].back() < arr[cur][j])
				{
					l = mid;
				}
				else
				{
					r = mid - 1;
				}
			}
			if(l >= 0 && l < i)
			{
				if(arr[id[l]].back() < arr[cur][j])
				{
					dp[i] = max(dp[i], dp[l] + (ll)arr[cur].size() - j);
				}
			}

		}
		//debug(i);
		//debug(dp[i]);
	}
	cout << dp[n - 1] << endl;
	
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





