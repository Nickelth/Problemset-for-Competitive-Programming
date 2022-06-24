[题目链接](https://codeforces.com/contest/1695/problem/C)

题意：

给出一个$n * m$的矩阵，矩阵中每个元素为$1$或$-1$，问是否存在从矩阵左上方走到右下方的道路（只能向下或向右走），使路上所有元素加和为$0$？

题解：

算法$1$:

首先，我们一共会经过$n + m - 1$个格子，所以$n + m - 1$必须为偶数.

令$Max_{i,j}$表示到格子$(i,j)$所能得到的最大值，$Min_{i,j}$表示到格子$(i,j)$所能得到的最小值，那么当
$Min_{n,m}\leq 0\leq Max_{n,m}$
时，一定有一条满足要求的路.

证明：

我们考虑将路径描述为$n - 1$个$D$和$m - 1$个$R$，其中$D$和$R$分别表示向下走和向右走；如果我们交换路径中的一对字符，那么路径上的和可能会$+2,+0,-2$，又由于$n + m - 1$是偶数，因此$1$和$-1$的数量要么都是奇数要么都是偶数，二者做差一定得到偶数，因此$Min_{n,m}$和$Max_{n,m}$一定都是偶数，故而我们从值为$Min_{n,m}$的路径不断交换其中两个字符，令其渐渐调整为值为$Max_{n,m}$的路径，其中一定会经过值为$0$的路径.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define INF 0x3f3f3f3f3f3f3f3f
using namespace std;
const ll N = 1e3 + 5;
ll n,m;
ll a[N][N];
ll mm[N][N];
ll mi[N][N];
void solve()
{
	cin >> n >> m;
	for(ll i = 1; i <= n; i ++ )
	{
		for(ll j = 1; j <= m; j ++ )
		{
			cin >> a[i][j];
			mi[i][j] = INF;
			mm[i][j] = -INF;
		}
	}
	
	if((n + m - 1) % 2 != 0)
	{
		cout << "NO" << endl;
		return;
	}
	mi[1][1] = a[1][1];
	mm[1][1] = a[1][1];
	for(ll i = 1; i <= n; i ++ )
	{
		for(ll j = 1; j <= m; j ++ )
		{
			if(i > 1){
				mi[i][j] = min(mi[i][j],mi[i - 1][j] + a[i][j]);
				mm[i][j] = max(mm[i][j],mm[i - 1][j] + a[i][j]);
			}
			if(j > 1)
			{
				mi[i][j] = min(mi[i][j],mi[i][j - 1] + a[i][j]);
				mm[i][j] = max(mm[i][j],mm[i][j - 1] + a[i][j]);
			}
			
		}
	}
	if(mi[n][m] <= 0 && 0 <= mm[n][m])
	{
		cout << "YES" << endl;
	}
	else
	{
		cout << "NO" << endl;
	}
}
int main()
{
	IOS;
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```

算法$2$:

将所有$-1$看作$0$，令$f[i][j][k]$表示到达$(i,j)$时能否凑出$k$个$1$，由于是$dp$值是布尔型，所以第三维可以用$bitset$来表示，$bitset$的第$i$位表示能否得到$i$个$1$.复杂度为$\frac{nm(n + m)}{w}$，其中$w$为机器字长.

这个做法可以扩展到一个更一般的问题：给出一个$n * m$的$01$矩阵，从左上角开始，只允许向右或向下走，到达右下角时能否得到$k$个$1$？

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define INF 0x3f3f3f3f3f3f3f3f
using namespace std;
const ll N = 1e3 + 5;
ll n,m;
ll a[N][N];
bitset<N>f[N][N];
void solve()
{
	cin >> n >> m;
	for(ll i = 1; i <= n; i ++ )
	{
		for(ll j = 1; j <= m; j ++ )
		{
			cin >> a[i][j];
			if(a[i][j] < 0)a[i][j] = 0;
			f[i][j].reset();
		}
	}
	
	if((n + m - 1) % 2 != 0)
	{
		cout << "NO" << endl;
		return;
	}
	f[0][1][0] = 1;
	for(ll i = 1; i <= n; i ++ )
	{
		for(ll j = 1; j <= m; j ++ )
		{
			f[i][j] = (f[i - 1][j] | f[i][j - 1]) << a[i][j]; 
			//注意bitset的状态定义，如果a[i][j]为1，那么如果原来能够得到i个1，这时能得到i + 1个1
			//所以原来的第i位现在要移动到第i + 1位 
		}
	}
	if(f[n][m][(n + m - 1) / 2])
	{
		cout << "YES" << endl;
	}
	else
	{
		cout << "NO" << endl;
	}
}
int main()
{
	IOS;
	ll _;
	cin >> _;
	while(_ -- )
	{
		solve();
	}
}
```


