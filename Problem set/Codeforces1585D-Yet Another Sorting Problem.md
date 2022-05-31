[题目链接](https://codeforces.com/contest/1591/problem/D)

题目大意：

给出一个数列，你可以每次选择三个下标不同的元素$i,j,k$，然后令元素$i$放到元素$j$的位置，元素$j$放到元素$k$的位置，元素$k$放到元素$i$的位置，问能否通过若干次这样的操作，使数列有序？

题解：

我们令$(i,j,k)$进行题目中的形如$i->j->k->i$的交换，实际上相当于发生了两次对换：先$(i,k)$对换，然后$(i,j)$对换，结果是，数列的奇偶性不会发生改变（排列的性质，数列也一样）；因此，数列可排序的充要条件是，数列是一个偶排列。

如果数列中有相同元素，那么我们总能通过对换两个相同元素来使奇偶性改变，因此总是有解。
    
代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
const ll N = 5e5 + 5;
const ll M = 100 + 5;
ll n;
ll a[N];
ll tmp[N];
ll merge_sort(ll l, ll r)
{
	if(l >= r)return 0;
	ll mid = (l + r) >> 1;
	ll res = 0;
	res = merge_sort(l,mid) + merge_sort(mid + 1, r);
	ll i = l, j = mid + 1;
	ll k = l;
	while(i <= mid && j <= r)
	{
		if(a[j] < a[i])
		{
			res += mid - i + 1;
			tmp[k ++ ] = a[j ++ ];
		}
		else
		{
			tmp[k ++ ] = a[i ++ ];
		}
	}
	while(i <= mid)tmp[k ++ ] = a[i ++ ];
	while(j <= r)tmp[k ++ ] = a[j ++ ];
	for(ll i = l; i <= r; i ++ )
	{
		a[i] = tmp[i];
	}
	return res;
}
int main()
{
	IOS;
	ll _;
	cin >> _;
	while(_ -- )
	{
		set<ll>s;
		cin >> n;
		for(ll i = 1; i <= n; i ++ )
		{
			cin >> a[i];
			s.insert(a[i]);
		}
		if(s.size() < n)
		{
			cout << "YES" << endl;
		}
		else
		{
			if(merge_sort(1,n) % 2 == 0)
			{
				cout << "YES" << endl;
			}
			else
			{
				cout << "NO" << endl;
			}
		}
	}
}
```
