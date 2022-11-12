[题目链接](https://codeforces.com/problemset/problem/1743/D)

题目大意：

给出一个随机生成的 $01$ 串，要求从中选取两个子串使二者的二进制或值最大.

题解：

直接暴力，我们只需要关注第一个全为 $1$ 的连续序列块；

关键在于注意到题目中说明每个字符分别有 $0.5$ 的概率生成 $1$ 或 $0$ ，那么从概率上来说，连续生成若干个 $1$ 的概率将会随长度指数变小，长度为  $30$ 时就几乎不可能了，所以可以暴力解决。

 代码：
 
 ```cpp
#include<bits/stdc++.h>
#define debug(x) cout << #x << " = " << (x) << endl
#define ll long long
using namespace std;
const ll N = 1e6 + 5;
ll n;
string s;
string cal(string a, string b)
{
	string res;

	for(ll i = 0; i < a.length(); i ++ )
	{
		if(i < a.length() - b.length())
		{
			res += a[i];
		}
		else
		{
			if(a[i] == '1' || b[i - (a.length() - b.length())] == '1')
			{
				res += '1';
			}
			else
			{
				res += '0';
			}
			
		}
	}
	return res;
}
int main()
{
	cin >> n >> s;
	ll f = -1;
	ll f2 = -1;
	for(ll i = 0; i < n; i ++ )
	{
		if(f == -1 && s[i] == '1')
		{
			f = i;
		}
		if(f != -1 && s[i] == '0')
		{
			f2 = i;
			break;
		}
	}
	if(f == -1)
	{
		cout << 0 << endl;
		return 0;
	}
	if(f2 == -1)
	{
		for(ll i = f; i < n; i ++ )
		{
			cout << s[i];
		}
		cout << endl;
		return 0;
	}
	ll len = n - f2;
	string ans;

	for(ll i = f; i < n; i ++ )
	{
		if(s[i] == '0')break;
		string cur = s.substr(i, len);
		if(ans.empty())
		{
			ans = cal(s, cur);
			continue;
		}

		if(cal(s, cur) > ans)
		{
			ans = cal(s, cur);
		}
		
	}
	f = 0;
	for(ll i = 0; i < n; i ++ )
	{
		if(ans[i] == '1')
		{
			f = 1;
		}
		if(f)
		{
			cout << ans[i];
		}
	}
	cout << endl;
	
}
```