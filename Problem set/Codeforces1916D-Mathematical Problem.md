[题目链接](https://codeforces.com/problemset/problem/1916/D)

题目大意：

给出一个数n，n为奇数，输出n个数，满足：

1.每个数都是平方数

2.每个数的数位的Multi-set相等，比如112233的数位Multi-set为{1,1,2,2,3,3}，和223311的数位Multi-set相等

n不超过99

题解：

在乘法运算中，答案的第n位等于 $\sum_{i + j = n}a_i*b_j$ ，其中 $a_i$ 和 $b_j$ 是两个乘数的数位；

n = 1时答案为{1};

n = 3时答案为{196, 169, 961}

n > 3时，可以先将每个n - 1时的答案乘以100，这样它们依然是平方数，然后再构造两个形如：

1...6...9, 9...6...1的数即可，省略号中间是 (n - 3) / 2 个0，这两个数分别是 1...3 和 3...1 的平方。

代码：

```c++ 
#include<bits/stdc++.h>
#define ll long long
#define ull unsigned long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll mod = 998244353;
const ll N = 2e5 + 5;
map<ll, vector<string> > ans;
int main()
{
	vector<string> vec;
	vec.push_back("1");
	ans[1] = vec;
	vec.clear();
	vec.push_back("169");
	vec.push_back("196");
	vec.push_back("961");
	ans[3] = vec;
	for(ll n = 5; n <= 99; n += 2)
	{
		vector<string> pre = ans[n - 2];
		vec.clear();
		for(auto i : pre)
		{
			string s = i;
			s += '0';
			s += '0';
			vec.push_back(s);
		}
		string s = "1";
		for(ll i = 0; i < (n - 3) / 2; i ++ )
		{
			s += "0";
		}
		s += "6";
		for(ll i = 0; i < (n - 3) / 2; i ++ )
		{
			s += "0";
		}
		s += "9";
		vec.push_back(s);
		reverse(s.begin(), s.end());
		vec.push_back(s);
		ans[n] = vec;
	}
	ll _;
	cin >> _;
	while(_ -- )
	{
		ll n;
		cin >> n;
		for(auto i : ans[n])
		{
			cout << i << endl;
		}
	}
}
```

