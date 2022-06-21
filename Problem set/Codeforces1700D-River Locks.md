[题目链接](https://codeforces.com/contest/1700/problem/D)

题目大意：给出$n$个水坝和每个水坝的容量，每个水坝上面有一个水管，能够以每秒$1$升的速度向水坝灌水.如果一个水坝满了，那么水会溢到其右边的水坝.进行$q$次询问，每个询问给出一个时间$t$，问最少打开多少个水管，可以在$t$秒内灌满所有水坝？


题解：

二分答案，然后检验开了$i$个水管后所需的最小时间是否不超过$t$；

显然在选择开哪些水管时，我们选择前$i$个水管，因为这些水管的水可以溢到后面的水管，防止造成浪费；

令$p_i$为前$i$个水坝的容量和，当开了$i$个水管时，需要的最小时间为$max\{ \lceil \frac{p_j}{j} \rceil \},1 \leq j \leq i$，之所以要取前缀中的最大值，是因为水管灌满水坝的时间有下限，比如第一个水坝最多只有一个水管给它灌水，所以时间最短为 $\frac{p_1}{1}$ ，前两个水坝最短时间是 $\frac{p_2}{2}$ ，以此类推.

我们可以把前缀的最短时间最大值预存起来，二分时进行判断即可.

代码：

```cpp
#include<iostream>
#include<algorithm>
#include<cstring>
#define ll long long
#define endl '\n'
#define debug(x) cout << #x << " = " << (x) << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define INF 0x3f3f3f3f3f3f3f3f
#define lson (p * 2)
#define rson (p * 2 + 1)
using namespace std;
const ll N = 2e5 + 5;
const ll mod = 998244353;
ll n,q,t;
ll v[N];
ll p[N];
void solve()
{
	ll l = 1, r = n;
	while(l < r)
	{
		ll mid = (l + r) / 2;
		if(mid * t >= p[n])
		{
			r = mid;
		}
		else
		{
			l = mid + 1;
		}
	}
	cout << l << endl;
}
int main()
{
	cin >> n;
	ll mm = 0;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> v[i];
		p[i] = p[i - 1] + v[i];
		mm = max(mm, (p[i] + i - 1) / i);
	}
	cin >> q;
	while(q -- )
	{
		cin >> t;
		if(t < mm)
		{
			cout << -1 << endl;
			continue;
		}
		solve();
	}
	
}
```

也可以不二分，直接计算得到答案.

```cpp
#include<iostream>
#include<algorithm>
#include<cstring>
#define ll long long
#define endl '\n'
#define debug(x) cout << #x << " = " << (x) << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define INF 0x3f3f3f3f3f3f3f3f
#define lson (p * 2)
#define rson (p * 2 + 1)
using namespace std;
const ll N = 2e5 + 5;
const ll mod = 998244353;
ll n,q,t;
ll v[N];
ll p[N];
int main()
{
	cin >> n;
	ll mm = 0;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> v[i];
		p[i] = p[i - 1] + v[i];
		mm = max(mm, (p[i] + i - 1) / i);
	}
	cin >> q;
	while(q -- )
	{
		cin >> t;
		if(t < mm)
		{
			cout << -1 << endl;
			continue;
		}
		else
		{
			cout << (p[n] + t - 1) / t << endl;
		}
	}
	
}
```



