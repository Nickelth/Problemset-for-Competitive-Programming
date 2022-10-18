[题目链接-简单版](https://codeforces.com/contest/1744/problem/E1)

[题目链接-困难版](https://codeforces.com/contest/1744/problem/E2)



题目大意：

给出四个数： $a$ , $b$ , $c$ , $d$ 满足 $a < c$ 且 $b < d$ .

求满足 $x \in (a,c]$ 、 $y \in (b,d]$ 且 $xy\ mod\ ab\ =\ 0$ 的 $x$ 和 $y$ .

题解：

首先对于简单版本，数据的范围都不超过 $10^5$ ，因此可以遍历 $x$ 的值，验证是否有满足条件的 $y$ 的值；

 $y$ 一定可以被 $\frac{ab}{gcd(ab,x)}$ 整除.
 
 证明：
 
 由于 $xy\ mod\ ab\ =\ 0$ ，故对 $ab$ 分解质因数，其所包含的质因数一定在 $xy$ 中出现过，且指数一定小于 $xy$ 中相应质因数的指数；
 
 那么， $gcd(ab,x)$ ，实际上就是从 $ab$ 的质因数集合中，取了被 $x$ 整除的那一部分子集；
 
 那么，为了令 $xy$ 能够整除 $ab$ ， $y$ 就必须能够整除剩下的质因数，也就是 $\frac{ab}{gcd(ab,x)}$ .
 
 所以我们只需要验证 $(b,d]$ 间是否有这个值的倍数即可.

代码（简单版）：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
ll a, b, c, d;
void solve()
{
	cin >> a >> b >> c >> d;
	
	for(ll i = a + 1; i <= c; i ++ )
	{
		ll m = a * b / __gcd(a * b, i);
		m = d / m * m;
		if(m > b)
		{
			cout << i << ' ' << m << endl;
			return;
		}
	}
	
	cout << "-1 -1" << endl;

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

对于困难版，范围扩大到了 $1e9$ .

考虑到，如果我们知道了 $gcd(ab,x)$ ，那么我们就可以直接解出 $y$ ，然后反过来解出 $x$ ；

由于 $gcd(ab,x)$ 是 $ab$ 的一个因数，所以，考虑遍历 $ab$ 所有的因数；

显然，直接对 $ab$ 求因数的话，复杂度过高；

有一个结论是， $ab$ 的任意因数都可以写成 $a^{'}b^{'}$ 的形式，其中 $a^{'}$ 是 $a$ 的因数， $b^{'}$ 是 $b$ 的因数；

另一个结论是，位数为 $9$ 的数字中因数最多的数有 $1344$ 个因数；[链接](https://oeis.org/A066150)

综合以上两条，我们可以选择分别遍历 $a$ 和 $b$ 的所有因数，然后进行组合；

代码（困难版）：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
ll a, b, c, d;
void get_fac(ll v, vector<ll>& fac)
{
	for(ll i = 1; i * i <= v; i ++ )
	{
		if(v % i == 0)
		{
			fac.push_back(i);
			
			if(i != v / i)
			{
				fac.push_back(v / i);
			}
		}
	}
}
void solve()
{
	cin >> a >> b >> c >> d;
	
	vector<ll>fac1,fac2;
	
	get_fac(a, fac1);
	get_fac(b, fac2);
	
	
	for(auto i : fac1)
	{
		for(auto j : fac2){
			ll g = i * j;
			ll y = a * b / g;
			y = d / y * y;
			
			if(y > b && y <= d)
			{
				ll x = a * b / __gcd(a * b, y);
				x = c / x * x;
				if(x > a && x <= c)
				{
					cout << x << ' ' << y << endl;
					return;
				}
			}
		}
	}
	
	cout << "-1 -1" << endl;

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

