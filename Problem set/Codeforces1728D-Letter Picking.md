[题目链接](https://codeforces.com/problemset/problem/1728/D)

题目大意：

给出一个长度为偶数的字符串$s$，$A$和$B$轮流从$s$的两头取走字符，然后将得到的字符放在自己的串的前面，以此分别构造属于自己的字符串.

$s$取空后，所得串字典序较小的一方获胜.

题解：

区间$dp$，令$dp_{l,r}$表示轮到$A$且串$s$剩余部分为原串的区间$[l,r]$时的胜负结果，$A$可能选左边也可能选右边，然后$B$总共也有四种相应的选择方式，枚举四种情况即可.

代码：

```cpp
#include<bits/stdc++.h>
#define ll int
#define IOS std::ios::sync_with_stdio(false),std::cin.tie(0),std::cout.tie(0)
#define debug(x) std::cout << #x << " = " << (x) << std::endl
const ll mod = 1e9 + 7;
const ll N = 2e3 + 5;
char s[N];
ll n;
ll dp[N][N];
void check_and_turn(ll& res, ll a, ll b)
{
	if(res == 2)
	{
		if(s[a] < s[b])
		{
			res = 0;
		}
		else if(s[a] > s[b])
		{
			res = 1;
		}
	}
}
ll dfs(ll l, ll r)
{

	if(dp[l][r] != -1)
	{
		return dp[l][r];
	}
	
	if(l >= r)
	{
		return dp[l][r] = -1;
	}
	
	if(r - l + 1 == 2)
	{
		
		if(s[l] == s[r])
		{
			return dp[l][r] = 2;
		}
		else
		{
			return dp[l][r] = 0;
		}
	}
	else
	{
		//A choose left 1st, B choose left 2nd
		ll res1 = dfs(l + 2, r);
		check_and_turn(res1, l, l + 1);
		
		//A choose left 1st, B choose right 1st
		ll res2 = dfs(l + 1, r - 1);
		check_and_turn(res2, l, r);
		
		//A choose right 1st, B choose right 2nd
		ll res3 = dfs(l, r - 2);
		check_and_turn(res3, r, r - 1);
		
		//A choose right 1st, B choose left 1st
		ll res4 = dfs(l + 1, r - 1);
		check_and_turn(res4, r, l);
		
		if(res1 == 0 && res2 == 0 || res3 == 0 && res4 == 0)
		{
			return dp[l][r] = 0;
		}
		else if(res1 != 1 && res2 != 1 || res3 != 1 && res4 != 1)
		{
			return dp[l][r] = 2;
		}
		else
		{
			return dp[l][r] = 1;
		}
		
		
	}
}
void solve()
{
	std::cin >> (s + 1);
	
	n = strlen(s + 1);
	
	for(ll i = 1; i <= n; i ++ )
	{
		for(ll j = 1; j <= n; j ++ )
		{
			dp[i][j] = -1;
		}
	}
	
	dfs(1, n);
	
	if(dp[1][n] == 0)
	{
		std::cout << "Alice" << '\n';
	}
	else if(dp[1][n] == 2)
	{
		std::cout << "Draw" << '\n';
	}
	else
	{
		std::cout << "Bob" << '\n';
	}
}
int main()
{
	IOS;
	ll _;
	std::cin >> _;
	
	while(_ -- )
	{
		solve();
	}
}
```
