[题目链接](https://atcoder.jp/contests/abc280/tasks/abc280_e)

题目大意：

有一只怪兽有 $n$ 点生命值，一次攻击有 $\frac{p}{100}$ 的概率造成 $2$ 点伤害，有 $1-\frac{p}{100}$ 的概率造成 $1$ 点伤害，问令怪兽生命值降到 $0$ 及以下的期望攻击次数？

题解：

解法 $1$ :

令 $a_i$ 表示造成伤害大于等于 $i$ 的期望攻击数， $a_{i + 1}$ 可能等于 $a_i$ 或 $a_{i} + 1$ . $a_{i + 1}$ 等于 $a_i + 1$ 当且仅当在某时刻造成的伤害恰好等于 $i$ ，假设造成伤害等于 $i$ 的概率为 $p_i$ ，则 $a_{i + 1} = a_i + p_i$ . 伤害不等于 $i$ 的事件是当伤害到达 $i - 1$ 时，下一击造成了 $2$ 点伤害，因此 $p_i = 1 -  p_{i - 1}\frac{p}{100}$ .

综上， $a_n = a_{n - 1} + p_{n - 1} = \sum_{i = 0}^{n - 1}p_i$ .

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define ld long double
using namespace std;
const ll MOD = 998244353;
ll n, p;
ll qmi(ll base, ll power)
{
	ll res = 1;
	while(power)
	{
		if(power & 1)
		{
			res = res * base % MOD;
		}
		power >>= 1;
		base = base * base % MOD;
	}
	return res;
}
int main()
{
	cin >> n >> p;
	ll cur = 1;
	ll ans = cur;
	for(ll i = 1; i <= n - 1; i ++ )
	{
		cur = (1 - cur * p % MOD * qmi(100, MOD - 2) % MOD) % MOD; 
		if(cur < 0)
		{
			cur += MOD;
			cur %= MOD;
		}
		ans += cur;
		ans %= MOD;
	}
	cout << ans << endl;
	 
}
```

解法 $2$ :

考虑最后一击 $K$，有两种可能性：

 $1$ . 第 $K$ 下刚好将血量降到 $0$ .

 $2$ . 第 $K - 1$ 下将血量降到 $1$ ，第 $K$ 下将血量降到 $-1$ .

对于第一种可能性， $\lceil \frac{n}{2} \rceil \leq K \leq n$ ，有 $n - K$ 下造成了 $2$ 点伤害，有 $2K - n$ 下造成了 $1$ 点伤害，那么概率就是 $C_{K}^{n-K}\frac{p}{100}^{n-K}(1-\frac{p}{100})^{2K-n}$ . 



对于第二种可能性， $\lceil \frac{n + 1}{2} \rceil \leq K \leq n$ ，除了最后一击，有 $n - K$ 下造成了 $2$ 点伤害，有 $2K - n - 1$ 下造成了 $1$ 点伤害，那么概率就是： $C_{K - 1}^{n-K}\frac{p}{100}^{n-K+1}(1-\frac{p}{100})^{2K-n-1}$ .

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define ld long double
using namespace std;
const ll MOD = 998244353;
const ll N = 2e5 + 5;
ll n, p;
ll fac[N];
ll inv[N];
ll mul(ll a, ll b)
{
	return (a % MOD) * (b % MOD) % MOD;
}
ll qmi(ll base, ll power)
{
	ll res = 1;
	while(power)
	{
		if(power & 1)
		{
			res = res * base % MOD;
		}
		power >>= 1;
		base = base * base % MOD;
	}
	return res;
}
ll C(ll m, ll n)
{
    if (n > m || n < 0)return 0;
    return mul(fac[m], mul(inv[n],inv[m - n]));
}
int main()
{
	cin >> n >> p;
	fac[0] = 1;
	inv[0] = 1;
	for(ll i = 1; i <= n; i ++ )
	{
		fac[i] = mul(fac[i - 1], i);
		inv[i] = qmi(fac[i], MOD - 2);
	}
	ll ans = 0;
	ll P = p * qmi(100, MOD - 2) % MOD;
	ll cP = 1 - P;
	if(cP < 0)
	{
		cP += MOD;
		cP %= MOD;
	}
	
	for(ll k = 1; k <= n; k ++ )
	{
		if(k >= n - k)
		{
			ans += mul(k, mul(C(k, n - k), mul(qmi(P, n - k), qmi(cP, 2 * k - n))));
			ans %= MOD;
		}
		if(k - 1 >= n - k)
		{
			ans += mul(k, mul(C(k - 1, n - k), mul(qmi(P, n - k + 1), qmi(cP, 2 * k - n - 1))));
			ans %= MOD;
		}
	}
	cout << ans << endl;
	 
}
```



