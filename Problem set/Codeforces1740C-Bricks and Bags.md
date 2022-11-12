[题目链接](https://codeforces.com/contest/1740/problem/C)

题目大意：

有 $n$ 个元素，每个元素有一个权值 $W_i$ ， $A$ 将这些元素分到三个口袋里，然后 $B$ 会从三个口袋各选择一个元素使得 $|W_1 - W_2|+ |W_2 - W_3|$ 最小，请问 $A$ 如何分配可以使得这个值最大？

题解：

首先对所有元素从小到大排序，设 $p_i$ 表示 $B$ 从口袋 $i$ 中取出的元素的下标，那么， 一共有 $4$ 种可能的情况：

1. $p_1 < p_2 < p_3$

2. $p_1 > p_2 > p_3$

3. $min(p_1,p_3) > p_2$

4. $p_2 > max(p_1,p_3)$

前两种一定不比后两种更优，因为交换其中任意两个元素可以得到更优的答案.

接下来以第三种情况为例进行分析：

设 $min(p_1,p_3) > p_2 + 1$ ，我们分别考虑元素 $p_2 + 1$ 在第一、二、三个口袋中的情况，发现无论哪一种， $B$ 选择该元素都可以令答案更小，因此一定有 $min(p_1,p_3) = p_2 + 1$ .

对于第四种情况，类似的，一定有 $max(p_1,p_3) = p_2 - 1$ .

对于第三种情况，我们令 $max(W_{p_1},W_{p_2})$ 最大可以使答案更优，即 $W_n$ ，然后枚举另外两个元素.

对于第四种情况，同理.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
ll n;
void solve()
{
	cin >> n;
	vector<ll>arr(n);
	for(ll i = 0; i < n; i ++ )
	{
		cin >> arr[i];
	}
	
	sort(arr.begin(), arr.end());
	

	ll ans1 = -1;
	ll ans2 = -1;

	for(ll i = 2; i < n; i ++ )
	{
		ans1 = max(ans1, arr[i] - arr[i - 1] + arr[i] - arr[0]);
	}

	for(ll i = 0; i < n - 2; i ++ )
	{
		ans2 = max(ans2, arr[i + 1] - arr[i] + arr[n - 1] - arr[i]);
	}
	
	cout << max(ans1, ans2) << endl;
	
	//debug(ans1);
	//debug(ans2);
	


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