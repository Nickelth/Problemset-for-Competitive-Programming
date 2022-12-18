[题目链接](https://atcoder.jp/contests/abc280/tasks/abc280_d)

题目大意：

给出一个不小于 $2$ 的整数 $K$ ，求最小的满足 $N!\ \% K\ = 0$ 的整数 $N$ .

题解：

二分查找满足条件的 $N$ ，计算 $p$ 的指数时使用勒让德定理.

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
using namespace std;
const ll N = 2e5 + 5;
ll k;
void get_fac(ll num, map<ll,ll>& fac)
{
	for(ll i = 2; i * i <= num; i ++ )
	{
		if(num % i == 0)
		{
			while(num % i == 0)
			{
				fac[i] ++ ;
				num /= i;
			}
			
		}
	}
	if(num > 1)
	{
		fac[num] ++ ;
	}
}
ll legendre(ll num, ll p)
{
	ll cnt = 0;
	ll tmp = p;
	while(num >= p)
	{
		cnt += num / p;
		p *= tmp;
	}
	return cnt;
}
int check(map<ll,ll>fac, ll cur)
{
	for(auto i : fac)
	{
		ll v = i.first;
		ll c = i.second;
		if(legendre(cur, v) < c)
		{
			return 0;
		}
	}
	return 1;
}
int main()
{
	cin >> k;

	map<ll,ll>fac;
	get_fac(k, fac);
	ll l = 2, r = k;
	while(l < r)
	{
		ll mid = (l + r) / 2;
		if(check(fac, mid)) r = mid;
		else l = mid + 1;
	}
	cout << r << endl;

	
}
```

