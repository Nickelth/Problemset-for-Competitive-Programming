[题目链接](https://codeforces.com/contest/1879/problem/D)

题目大意：

给出一个 n 个数的数列 {a} ，求：
$$
\sum_{i=1}^{n}\sum_{j=1}^{n}f(l,r)\times(r - l + 1), f(l,r)=a_l\bigoplus a_{l+1} \bigoplus ... \bigoplus a_{r}
$$
题解：

首先考虑按每一位分别求值，然后加和，这样问题转换为01串上的问题

然后，固定r，考虑对每个r计算其答案。

令prei=1表示前i个数（指01串）异或为1，prei=0表示异或为0；

那么固定r求所有f(l,r)也就是求r之前有多少l - 1满足pre(l-1) + pre(r) = 1

令满足上式的pre(l-1)的数量为cnt，l - 1加和的值为sum

则所有r-l+1的和为r * cnt - sum

记得这是当前这一位的答案，需要倍乘后加入总答案

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define debug(x) cout << #x << " = " << (x) << endl
#define pll pair<ll,ll>
using namespace std;
const ll N = 3e5 + 5;
const ll M = 998244353;
ll n;
ll a[N];
ll add(ll t1, ll t2)
{
	t1 += t2;
	
	t1 %= M;
	if(t1 < 0) t1 += M;
	return t1;
}
ll mul(ll t1, ll t2)
{
	t1 *= t2;
	t1 %= M;
	return t1;
}
int main()
{
	cin >> n;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
	}
	
	ll ans = 0;
	for(ll i = 0; i < 30; i ++ )
	{
		ll cnt[2] = {0};
		ll sum[2] = {0};
		cnt[0] = 1;
		ll x = 0;
		ll cur = 0;
		for(ll j = 1; j <= n; j ++ )
		{
			x ^= ((a[j] & (1ll << i)) != 0);
			cur += add(mul(cnt[1 ^ x], j), -sum[1 ^ x]);
			cur %= M;
			cnt[x] = add(cnt[x], 1ll);
			sum[x] = add(sum[x], j);
			
		}
		ans = add(ans, mul(1ll << i, cur));
	}
	cout << ans << endl;
	
}
```

