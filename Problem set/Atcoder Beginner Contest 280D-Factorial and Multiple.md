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

第二种方法（不需要勒让德定理）：

设 $f_p(x)=a$ 表示 $x$ 是 $p_a$ 的倍数，但不是 $p_{a+1}$ 的倍数，对每个指数为 $a_i$ 的质因子 $p_i$ ，我们要找到一个 $N_i$ 满足

 $f_{p_i}((N_i-1)!) < a_i \leq f_{p_i}(N_i!)$ .

由于 $f_p(N!) = f_p((N-1)!) + f_p(N)$ ，即 $f_p(N)>0$ ，则 $N$ 是 $p$ 的倍数；因此，由 $f_{p_i}(p_i!)=1$ 和 $f_{p_i}(N_i!)=f_{p_i}((N_i-p_i)!)+f_{p_i}(N_i)$ ，对每个 $N_i=p_i,2p_i,...$ 进行计算.

这样对每个 $p_i$ 得到一个 $N_i$ ，从这些 $N_i$ 中选择一个最大的即可，因为若 $N_i > N_j$ ，那么前者的阶乘一定是后者阶乘的倍数.

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
using namespace std;
const ll N = 2e5 + 5;
ll k,p,a,n,x,ans;
int main()
{
	cin >> k;
	ans = 1;
	for(p = 2; p * p <= k ; p ++ )
	{
		a = 0;
		while(k % p == 0)
		{
			k /= p; 
			a ++ ;
		}
		n = 0;
		while(a > 0)
		{
			n += p;
			x = n;
			while(x % p == 0)
			{
				x /= p;
				a -- ;
			}
		}
		ans = max(ans, n);
	}
	ans = max(ans, k);
	cout << ans << endl;
	
}
```

