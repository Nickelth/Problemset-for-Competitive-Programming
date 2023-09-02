[题目链接](https://codeforces.com/problemset/problem/1852/B)

题目大意：

给出一个包含n个非负整数的数列a，构造一个与之对应的数列b，满意：

b中所有数绝对值不相同

b中数绝对值在1到n之间

如果  $a_i = k$ ，那么b中必须恰好有 k 个数，与 $b_i$ 相加大于 0

题解：

问题等价于从数对(n, -n), (n-1, -(n-1)), ... ,  (1, -1)，每对中选择一个数，分配给b。

考虑b中绝对值最大的数，该数对应的 $a_i$ 只有两种可能，0或者n，如果为0， $b_i$ 为负数，否则为正数。并且不可能有其他同时等于0和n的数，否则产生矛盾，不可能构造出b。所以绝对值最大的 $b_i$ ，可以由此确定其值。

那么，去掉 $b_i$ ，令a中所有数减去已经移除的b中大于0的数的个数，问题就降为n-1的规模，继续上述步骤即可。

对a进行排序，每次对头和尾的数进行判断和移除，可以将该过程优化为O(n)

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 1e5 + 5;
ll n;
pll a[N];
void solve()
{
	
	vector<pll> b;
	ll l = 1;
	ll r = n;
	ll id = n;
	ll pos = 0;
	while(l <= r)
	{
		pll cur;
		
		if(a[l].first - pos == 0 && a[r].first - pos == r - l + 1)
		{
			cout << "NO" << endl;
			return;
		}
		else if(a[l].first - pos == 0)
		{
			cur.second = a[l].second;
			cur.first = -id;
			id -- ;
			l ++ ;
		}
		else if(a[r].first - pos == r - l + 1)
		{
			cur.second = a[r].second;
			cur.first = id;
			id -- ;
			r -- ;
			pos ++ ;
		}
		else
		{
			cout << "NO" << endl;
			return;
		}
		b.push_back(cur);
	}
	sort(b.begin(), b.end(), [&](pll t1, pll t2)
	{
		return t1.second < t2.second;
	});
	cout << "YES" << endl;
	for(auto i : b)
	{
		cout << i.first << ' ';
	}
	cout << endl;
	
}
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		cin >> n;
		for(ll i = 1; i <= n; i ++ )
		{
			cin >> a[i].first;
			a[i].second = i;
		}
		sort(a + 1, a + 1 + n);
		solve();
	}
}
```

