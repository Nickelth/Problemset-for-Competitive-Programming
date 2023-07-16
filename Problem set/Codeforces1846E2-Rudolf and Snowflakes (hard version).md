[题目链接](https://codeforces.com/contest/1846/problem/E2)

题目大意：

给出一个数，问该数能否表示为 $k^0 + k^1 + ... + k^p$ ， $p >= 2$ 。

题解：

预处理出所有可以表示为该形式的数即可。

注意到数据范围为 $1e18$ ，直接枚举会超时。

因此，只预处理指数 $3 <= p <= 63$ 的数，底数的范围可被限制为 $\sqrt[3] (n)$ ，不会超时。

而对于 $p = 2$ ，只需看方程 $k^2 + k + 1 = n$ 有没有解即可。

代码：

```c++
#include<bits/stdc++.h>
#define debug(x) std::cout << #x << " = " << (x) << '\n';
using ll = long long;
const ll N = 1e6;
ll n;
std::set<ll> ok;
int check(ll x)
{

	if(x == 1 || x == 2) return 0;
	if(ok.count(x) != 0) return 1;
	if(4 * x > 3 && (ll)sqrt(4 * x - 3) * (ll)sqrt(4 * x - 3) == (4 * x - 3) && ((ll)(sqrt(4 * x - 3) - 1) % 2 == 0 ) && ((ll)(sqrt(4 * x - 3) - 1) / 2 > 1))
	{
		return 1;
	}
	return 0;
}
void init()
{
	for(ll base = 2; base <= N; base ++ )
	{
		ll sum = 1 + base;
		ll cur = base * base;
		for(ll power = 3; power <= 63; power ++ )
		{
			sum += cur;
			if(sum > 1e18) break;
			ok.insert(sum);
			
			if(cur > (ll)(1e18) / base) break;
			cur *= base;
		}
	}
}
void solve(ll n)
{
	if(check(n) == 1) std::cout << "YES\n";
	else std::cout << "NO\n";
}
int main()
{
	std::ios::sync_with_stdio(false);
	std::cin.tie(0);
	std::cout.tie(0);
	ll _;
	std::cin >> _;
	init();
	while(_ -- )
	{
		std::cin >> n;
		solve(n);
	}

}


```

