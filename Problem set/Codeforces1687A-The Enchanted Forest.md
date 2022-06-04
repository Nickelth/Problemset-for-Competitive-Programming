[题目链接](https://codeforces.com/contest/1688/problem/D)

题目大意：

给出一个长度为$n$数列，每一秒每个元素的值会加一，你可以以任意元素为起点，在接下来的$k$秒，每一秒中依次执行以下操作：

走向相邻元素或原地不动；

收集当前位置的值，收集后这里的值变为0；

所有元素的值加一；

求能收集到的最大值；

题解：

分类讨论。

当$k <= n$时，显然只需要找值最大的长度为$k$的区间即可；

当$k > n$时，注意到对于一个元素，分几次收割和只在最晚的一秒收割一次效果是一样的，所以最优操作是先原地不动，等到最后剩余$n$秒时走一遍数列，把元素都收割一遍。

题外话：

这一场的题有些考验思维灵活性，所以$C$和$D$都没能做出来。和$C$类似，$D$我也注意到了“分几次收割和最后收割效果一样”这个关键点，但仍然没想到等到最后$n$秒再收割。下一次遇到这种问题该如何考虑呢？有一些可用的经验：
    
1.分类讨论永远是一个简化问题最简便的方式，不必一开始就试图想一个很“周全”的算法；

2.对于这类可以自己选择操作顺序/方式的问题，先尝试考虑是否存在一种通用的、简单的操作方式。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define ull unsigned long long
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
#define endl '\n'
const ll N = 2e5 + 5;
ll n,k;
ll a[N];
ll p[N];
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		cin >> n >> k;
		for(ll i = 1; i <= n; i ++ )
		{
			cin >> a[i];
			p[i] = p[i - 1] + a[i];
		}
		if(k <= n)
		{
			ll ans = 0;
			for(ll i = 1; i + k - 1 <= n; i ++ )
			{
				ans = max(ans, p[i + k - 1] - p[i - 1]);
			}
			ans += k * (k - 1) / 2;
			cout << ans << endl;
		}
		else
		{
			ll ans = p[n];
			
			ans += (k - 1 + k - n) * n / 2;

			cout << ans << endl;
		}
	}
}
```