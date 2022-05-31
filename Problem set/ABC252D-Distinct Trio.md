[题目链接](https://atcoder.jp/contests/abc252/tasks/abc252_d)

题目大意：

给出一个数列，求满足$i < j < k$且$A_i,A_j,A_k$不相同的三元组的数量。

题解：

原问题等价于：求所有满足$A_i < A_j < A_k$的三元组$(i,j,k)$的数量，注意$i,j,k$没有相对大小要求，枚举$A_j$然后使用前缀和统计即可。

代码：

```cpp
#include<iostream>
#include<set>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
const ll N = 2e5 + 5;
const ll M = 100 + 5;
ll n;
ll a[N];
ll p[N];
int main()
{
	IOS;
	cin >> n;
	set<ll>s;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
		p[a[i]] ++ ;
		s.insert(a[i]);
	}
	for(ll i = 1; i < N; i ++ )
	{
		p[i] += p[i - 1];
	}
	ll ans = 0;
	for(auto i : s)
	{
		ans += (p[i] - p[i - 1]) * p[i - 1] * (p[N - 1] - p[i]);
	}
	cout << ans << endl;
	
}
```