[题目链接](https://ac.nowcoder.com/acm/contest/33186/I)

题目大意：

一副牌一共有34种，每种4张，目前玩家手里有13张，且没有同一种牌的数量大于两张；

每轮该玩家从牌堆里取一张，如果凑成7个对子，则获胜，否则弃一张牌;

给出初始的手牌组合，问获胜的期望轮数是多少轮？

题解：

期望 $DP$ .

由于一开始手中同种牌的数量不大于2，不难发现，答案取决于手里有多少张单牌以及牌堆里剩多少牌.

最优策略是，如果摸到和手里的单牌凑对的牌，就弃一张单牌，否则弃掉取到的牌.

令 $F_{i,j}$ 表示手里有 $i$ 张单牌，牌堆里有 $j$ 张牌时的获胜期望轮数，有：

$$
F_{i,j} = \frac{3}{j} * 1 + \frac{j - 3}{j} * (1 + F_{1,j - 1}),\ i = 1 
$$
$$
F_{i,j} = \frac{3i}{j} * (1 + F_{i - 2,j - 1}) + \frac{j - 3i}{j} * (1 + F_{i,j - 1}),\ i > 1
$$




代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll mod = 1e9 + 7;
ll testcase = 1;
map<string, ll> mp;
string str;
ll dp[20][200];
ll inv[200];
ll qmi(ll base, ll power)
{
	ll res = 1;
	while(power)
	{
		if(power & 1)
		{
			res = (res * base) % mod;
		}
		power >>= 1;
		base = (base * base) % mod;
	}
	return res;
}
ll mul(ll a, ll b)
{
	return (a % mod) * (b % mod) % mod;
}
void solve()
{
	for(ll i = 0; i <= 13; i ++ )
	{
		for(ll j = 0; j <= 136; j ++ )
		{
			dp[i][j] = 0;
		}
	}
	
	dp[1][1] = dp[1][2] = dp[1][3] = 1;
	for(ll i = 4; i <= 34 * 4 - 13; i ++ )
	{
		dp[1][i] = mul(3,inv[i]) + mul(mul(i - 3, inv[i]), 1 + dp[1][i - 1]);
		dp[1][i] %= mod; 
	}
	
	for(ll i = 2; i <= 14; i ++ )
	{
		dp[i][1] = dp[i][2] = dp[i][3] = 1;
		for(ll j = 4; j <= 34 * 4 - 13; j ++ )
		{
			dp[i][j] = mul(mul(mul(3,i), inv[j]), 1 + dp[i - 2][j - 1])
						+ mul(mul(j - mul(3,i), inv[j]), 1 + dp[i][j - 1]);
			dp[i][j] %= mod;
		}
	}
	
}
void init()
{
	inv[0] = 1;
	for(ll i = 1; i <= 136; i ++ )
	{
		inv[i] = qmi(i, mod - 2);
	}
}
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	init();
	solve();
	ll _;
	cin >> _;
	while(_ -- )
	{
		cin >> str;
		mp.clear();

		for(ll i = 0; i < str.length(); i += 2)
		{
			string s;
			s += str[i];
			s += str[i + 1];
			mp[s] ++ ;
		}
		
		ll cnt = 0;
		for(auto i : mp)
		{
			if(i.second == 1)
			{
				cnt ++ ;
			}
		}
	
		ll tot = 34 * 4 - 13;
		cout << "Case #" << testcase ++ << ": ";
		cout << dp[cnt][tot] << endl;
	}
}
```