[题目链接](https://codeforces.com/problemset/problem/1730/B)

题目大意：

有 $n$ 个人分布在一条横轴上，每个人从其所在的位置 $x_i$ 到另一个位置时，首先需要花费 $t_i$ 的时间打扮，然后花费等于起点到目标点间距离的时间到达目标点.

请给出轴上一点，使得 $n$ 个人到该点的时间的最大值最小.

题解：

两种解法.

解法1：

对一点 $(x_i,t_i)$ 可以分裂为两个点 $(x_i - t_i,0)$ 和 $(x_i + t_i,0)$ ，然后求轴上最左最右端点坐标的中点即可.

证明：

假设答案选取在 $x_0$ 处，$x_0 < x_i$ ，那么点 $(x_i,t_i)$ 到 $x_0$ 的时间是 $t_i + x_i - x_0$ .

分裂成两个点后，两点到 $x_0$ 的距离分别是 $|x_i - x_0 - t_i|$ 和 $x_i + t_i - x_0$ ，后一个值和原来的点的时间一样，而前一个值不会大于后一个值，因此二者取极大值仍然是后一个值，与原来的点一样.

代码1：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define ld long double
#define pll pair<ll,ll>
#define debug(x) cout << #x << " = " << (x) << endl
#define eps 1.0E-6
using namespace std;
const ll N = 2e5 + 5;
ll n;
vector<pll>arr;

void solve()
{
	cin >> n;

	arr.clear();
	arr.resize(n);
	for(ll i = 0; i < n; i ++ )
	{
		cin >> arr[i].first;
	}
	for(ll i = 0; i < n; i ++ )
	{
		cin >> arr[i].second;
	}
	ll l = 1e9 + 5;
	ll r = 0;
	for(auto i : arr)
	{
		l = min(l, i.first + i.second);
		l = min(l, i.first - i.second);
		r = max(r, i.first + i.second);
		r = max(r, i.first - i.second);
	}
	cout << ((ld)l + (ld)r) / 2 << endl;
	
}
int main()
{
	ll _;
	cin >> _;
	cout << fixed << setprecision(12);
	while(_ -- )
	{
		solve();
	}
}
```

解法2：

由于花费的时间具有单调性，考虑二分最终花费的时间 $T$ .

在时间 $T$ 内，点 $(x_i,t_i)$ 所能到达的区间为 $[x_i - (T - t_i),x_i + (T - t_i)]$ ，$T$ 可以成为答案当且仅当所有点的可达区间有交集.

代码2：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define ld long double
#define pll pair<ld,ld>
#define debug(x) cout << #x << " = " << (x) << endl
#define eps 1.0E-6
using namespace std;
const ll N = 2e5 + 5;
ll n;
vector<pll>arr;

int check(ld T)
{
	//debug(T);
	vector<pll>seg;
	ld L = 0;
	ld R = 1e9;
	for(auto i : arr)
	{
		ld l = i.first - (T - i.second);
		ld r = i.first + (T - i.second);
		seg.push_back({l, r});
		L = max(L, l);
		R = min(R, r);
	}
	//debug(L);
	//debug(R);
	if(L <= R)
	{
		return 1;
	}
	else
	{
		return 0;
	}
	
}
void solve()
{
	cin >> n;

	arr.clear();
	arr.resize(n);
	for(ll i = 0; i < n; i ++ )
	{
		cin >> arr[i].first;
	}
	for(ll i = 0; i < n; i ++ )
	{
		cin >> arr[i].second;
	}
	sort(arr.begin(),arr.end());

	ld l = 0, r = 1e9;
	while(r - l > eps)
	{
		ld mid = (l + r) / 2;
		if(check(mid))
		{
			r = mid;
		}
		else
		{
			l = mid;
		}
	}

	double L = -1e9;
	double R = 1e9;
	for(auto i : arr)
	{
		double lb = i.first - (l - i.second);
		double rb = i.first + (l - i.second);
		L = max(L, lb);
		R = min(R, rb);

	}


	cout << fixed << setprecision(12) << ((double)L + (double)R) / 2 << endl;
	
}
int main()
{
	
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	ll _;
	cin >> _;
	
	while(_ -- )
	{
		solve();
	}
}
```



