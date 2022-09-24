[题目链接](https://codeforces.com/problemset/problem/1725/H)

题目大意：

定义 $cancat(a,b)$ 为两个数的连接，比如 $concat(12,34) = 1234$ .

有 $n$ 个数字， $n$ 为偶数，需要将这些数字均分到两个集合里，并选定一个数 $Z, 0 \leq Z < 3$ ，使得对任意两个来自不同集合的数 $A_i,A_j$ ，都不存在 $concat(A_i,A_j) * concat(A_j,A_i) + A_i * A_j \equiv Z\ mod\  3$ .

题解：

设 $D_a$ 表示数字 $a$ 的位数，注意到 $concat(A_i,A_j) = A_i * 10^{D_{A_j}} + A_j$ ，又有 $10^{k} \equiv 1\ mod\ 3$ ，故式  $concat(A_i,A_j) * concat(A_j,A_i) + A_i * A_j \equiv Z\ mod\  3$ 可化为：

$(A_{i}^{2} + A_{j}^{2}) \equiv Z\ mod \ 3)$ 

$A^{2}\ mod\ 3$ 只有三种情况：

$0^{2}\ \%\ 3 = 0$ 

$1^{2}\ \%\ 3 = 1$

$2^{2}\ \%\ 3 = 1$

因此枚举 $Z$ 的取值并分别考察三种情况即可.

当然，也有更巧妙的构造方法.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const ll N = 1e5 + 5;
ll n;
ll a[N];
int main()
{
	cin >> n;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
	}
	
	string ans(n, 'x');
	
	
	ll cnt[3] = { 0 };
	
	for(ll i = 1; i <= n; i ++ )
	{
		cnt[a[i] * a[i] % 3] ++ ;
	}
	
	ll z = 0;
	
	if(cnt[0] <= n / 2)
	{
		ll sum1 = 0;
		ll sum2 = 0;
		for(ll i = 1; i <= n; i ++ )
		{
			if(a[i] * a[i] % 3 == 0)
			{
				ans[i - 1] = '0';
				sum1 ++ ;
			}
			else if(sum2 < n / 2)
			{
				ans[i - 1] = '1';
				sum2 ++ ;
			}
			else
			{
				ans[i - 1] = '0';
				sum1 ++ ;
			}
		}
		cout << z << endl << ans << endl;
		return 0;
	}
	
	z = 1;
	
	if(cnt[0] == 0 || cnt[1] == 0 || cnt[0] + cnt[1] <= n / 2)
	{
		ll sum1 = 0;
		ll sum2 = 0;
		
		if(cnt[0] != 0 && cnt[1] != 0) {
			
			for(ll i = 1; i <= n; i ++ )
			{
				if(a[i] * a[i] % 3 == 0 || a[i] * a[i] % 3 == 1)
				{
					ans[i - 1] = '0';
					sum1 ++ ;
				}
				else if(sum2 < n / 2)
				{
					ans[i - 1] = '1';
					sum2 ++ ;
				}
				else
				{
					ans[i - 1] = '0';
					sum1 ++ ;
				}
			}
		}
		else
		{
			for(ll i = 1; i <= n; i ++ )
			{
				if(i <= n / 2)
				{
					ans[i - 1] = '0';
				}
				else
				{
					ans[i - 1] = '1';
				}
			}
		}
		cout << z << endl << ans << endl;
		return 0;
	}
	
	z = 2;
	
	if(cnt[1] <= n / 2)
	{
		ll sum1 = 0;
		ll sum2 = 0;
		
		for(ll i = 1; i <= n; i ++ )
		{
			if(a[i] * a[i] % 3 == 1)
			{
				ans[i - 1] = '0';
				sum1 ++ ;
			}
			else if(sum2 < n / 2)
			{
				ans[i - 1] = '1';
				sum2 ++ ;
			}
			else
			{
				ans[i - 1] = '0';
				sum1 ++ ;
			}
		}
		cout << z << endl << ans << endl;
		return 0;
	}
	
	cout << -1 << endl;
}
```

