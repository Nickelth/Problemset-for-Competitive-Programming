[题目链接](https://codeforces.com/contest/1765/problem/D)

题目大意：

假设有 $n$ 部影片，每部影片的大小是 $a_i$ ，需要 $a_i$ 单位的时间下载；

有一个大小为 $m$ 的磁盘来存储影片，只有当磁盘空余大小大于等于要下载的影片大小才能开始下载；

观看一部影片的耗时是 $1$ 个时间单位，看完后可以删除影片腾出空间，删除不花费时间；

问如何安排影片的下载和观看顺序，可以令看完所有影片的耗时最短？

题解：

这题的证明其实我没完全理解.

对于一对影片 $(a_i,a_j)$ ，显然当 $a_i + a_j <= m$ 时，可以只花费 $a_i + a_j$ 的时间完成下载与观看，那么，为了让总时间尽量短，就应该让这样的数对尽量多.

假设数组 $A$ 升序，一种最佳的排序方式是 $a_n,a_1,a_{n-1},a_2,...$ [1].

所以我们可以寻找一个子数组，满足：经过上面这种方式排序后，该子数组不存在 $a_i + a_j > m$ 的相邻数对.

子数组的左边界固定为 $a_1$ ，右边界可以使用二分枚举.

[1]这个证明我并没有理解其正确性，感性地理解一下应该是这样：

假设数组重排后的最大数对是 $V = a_j + a_{n-j+1}$ , $j < n-j+1$ ，那么数组中有至多 $j-1$ 个小于 $a_j$ 的值（称为集合 $A$ ），以及至少 $j$ 个大于等于 $a_j$ 的值（称为集合 $B$ ），为了得到比 $V$ 更小的值，就需要将 $B$ （ $2j - 2$ 条边）与 $A$ （ $2(j-1)$ 条边）匹配，两个集合匹配后，将任何剩下没有匹配的值尝试插入进去，都会得到更大的值.

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
ll n, m;
bool can(ll x, vector<ll>&a)
{
	ll l = 0, r = x - 1;
	while(l < r)
	{
		if(a[l] + a[r] > m)return false;
		r -- ;
		if(l < r)
		{
			if(a[l] + a[r] > m)return false;
			l ++ ;
		}
	}
	return true;
}
int main()
{
	
	cin >> n >> m;
	vector<ll>a(n);
	ll sum = 0;
	for(ll i = 0; i < n; i ++ )
	{
		cin >> a[i];
		sum += a[i];
	}
	sort(a.begin(), a.end());
	ll l = 1, r = n;
	while(l < r)
	{
		ll mid = (l + r + 1) / 2;
		if(can(mid,a)) l = mid;
		else r = mid - 1;
	}
	cout << sum + n - l + 1 << endl;
	
}
```





