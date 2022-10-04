[题目链接](https://codeforces.com/problemset/problem/1721/D)

非常巧妙的问题

题目大意：

给出两个数列 $A$ 和 $B$ ，你可以随意改变 $B$ 的顺序，求 $(A_1\ xor\ B_1)\ \&\ (A_2\ xor\ B_2)\ \&\ ...\ \&\ (A_n\ xor\ B_n)$ 的最大值.

题解：

从高位到低位求解.

一个直观的想法是，对某一位，如果 $A$ 所有元素在该位上为 $0$ 的数量等于 $B$ 所有元素在该位上为 $1$ 的数量，且 $A$ 所有元素在该位上为 $1$ 的数量等于 $B$ 所有元素在该位上为 $0$ 的数量，那么答案 $ans$ 在该位上可以置 $1$ .

但是，在考虑低位时，无法与高位独立开来分别考虑，因为低位直接按上面去配对，有可能打乱之前高位的配对方式.

如何在检查低位时，同时也考虑到高位呢？

假设，有一个 $A$ 数组，所有元素的最高位是 $[1,1,1,0,0,0]$ ，如果数组 $B$ 所有元素最高位是 $[0,0,0,1,1,1]$ ，那么接下来我们检查第二位时，自然希望将前三个元素分为一组，后三个元素分为一组，每一组内分别去检查第二位的情况，然后汇总起来. 检查第三位的时候，也是一样的思路.

如何完成这样的分组呢？

我们可以使用掩码来做到这一点.

注意到 $(A_i\ \&\ ans)\ xor\ (B_i\ \&\ ans)\ =\ ans\ \ \ \ (1)$ . 

我们从高位遍历到低位，对每一位，令 $ans$ 该位为 $1$ ，然后按 $A_i \& ans$ 对 $A$ 分组，如果每一组的元素恰好有等量的、满足式 $(1)$ 的 $B_i \& ans$ 存在，那么 $ans$ 该位可以置 $1$ ，否则置 $0$ . 

构造一些例子可以帮助理解，这个算法如何成功地将两个数组一步步分组，最终得到我们需要的答案.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 1e5 + 5;
ll n;
ll a[N];
ll b[N];
void solve()
{
	cin >> n;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
	}
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> b[i];
	}
	ll ans = 0;
	auto check = [=](ll ans)->int
	{
		map<ll,ll>mp;
		for(ll i = 1; i <= n; i ++ )
		{
			mp[a[i] & ans] ++ ;
		}
		for(ll i = 1; i <= n; i ++ )
		{
			mp[~b[i] & ans] -- ;
		}
		for(auto i : mp)
		{
			if(i.second != 0)return 0;
		}
		return 1;
	};
	for(ll i = 29; i >= 0; i -- )
	{
		if(check(ans | (1 << i)))
		{
			ans |= (1 << i);
		}
	}
	cout << ans << endl;
	
}
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```