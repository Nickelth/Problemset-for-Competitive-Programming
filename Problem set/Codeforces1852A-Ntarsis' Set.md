[题目链接](https://codeforces.com/contest/1852/problem/A)

题目大意：

给出一个数列 $[a_1, a_2, a_3, ... ,a_n]$ ，对于所有自然数，每次删除第$a_1, a_2, ... , a_n$ 个，执行 k 次后，剩下的最小的数是多少？

题解：

对于 $a_i, a_{i + 1}$ 之间的数x，执行一次删除后其位置变为x-i，可以从这一点入手，反向还原出k轮后x的位置。

我们将数列{a}转换为一个新的数列{seg}， $seg_i$ 表示第 i 个连续区间，将第 i 个连续区间的最小数设为 $fir_i$ 。

在第k轮，x的位置一定在1，假设当前轮次x的位置在pos，且pos 位于seg[i], seg[i + 1]间，那么只要pos + i < fir[i + 1]，下一轮pos一定会变成pos + i，直到pos + i >= fir[i + 1]，在pos > a[n]后，则一直是pos + n。

另外，有没有一种可能，pos + i 到达某个位置，而这个位置已经被之前的操作删了？答案是不会，考虑seg[1], seg[2]，第一次删seg[1], seg[2], 第二次从seg[1] + 1、seg[2] + seg[1] + 1开始删，所以seg[2] + 1 到 seg[2] + seg[1]之间的数不会被影响。

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define INF 0x3f3f3f3f3f3f3f3f
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
ll n, k;
ll a[N];
ll fir[N];
void solve()
{
	cin >> n >> k;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
	}
	if(a[1] != 1)
	{
		cout << 1 << endl;
		return;
	}

	ll cur = 1;
	vector<ll> seg;
	ll cnt = 1;
	fir[0] = 1;
	for(ll i = 2; i <= n; i ++ )
	{
		if(a[i] != a[i - 1] + 1)
		{
			seg.push_back(cnt);
			cnt = 1;
			fir[seg.size()] = a[i];
		}
		else
		{
			cnt ++ ;
		}
	}
	seg.push_back(cnt);
	fir[seg.size()] = INF;
	for(ll i = 1; i < seg.size(); i ++ )
	{
		seg[i] += seg[i - 1];
	}
	ll ans = 1;
	ll i = 0;
	ll j = 0;
	while(i < k)
	{
		while(i < k && ans + seg[j] < fir[j + 1])
		{
			ans += seg[j];
			i ++ ;
		}
		if(j + 1 < seg.size() && ans + seg[j] >= fir[j + 1]) j ++ ;
	}
	cout << ans << endl;
	
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

