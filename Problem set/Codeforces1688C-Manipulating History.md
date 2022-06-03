[题目链接](https://codeforces.com/problemset/problem/1688/C)

题目大意：

给出一个字符串，一开始字符串长度只有一，每次可以选择一个子串，换成另一个串，最后得到一个结果串。

现在给出这个过程中被选择的子串、和子串换得的串，但是顺序与操作顺序不同，同时给出结果串，求初始字符串。

题解：

考虑逆过程，发现整个过程是：选一次串换一次串选一次串换一次串……最后得到一个字母，在这个过程中如果我们把结果串也考虑进去，那么除了最后的字母，对其他每个字母的操作都是成对的，也就是其他每个字母都出现了偶数次，而最后的字母因为少一次操作，所以只有它出现了奇数次，因此只需要统计各字母出现次数，然后输出奇数的那个即可。

题外话：

我已经很多次在这种"ad-hoc"问题上吃过亏了，但我仍然没有找到应对这种问题的好方法。实际上，在这个问题的思考过程中，我已经发现了“成对”这个要素，但是考虑的对象是“子串”，所以走入了死胡同。是否有方法能够让我跳出固有思维？至少回顾这个问题的思考过程，我能想到的仅仅是两个或许可以提供帮助的方法：

1.多构造一些小的样理。如果当时我多构造一些小的样例，或许我会更明显地发现这个规律？

2.从更小的元素的角度考虑问题。许多问题要求我从单个的元素角度考虑问题，比如一些计数问题，直接计数可能很困难，往往需要我考虑单个元素对答案的贡献。这题也一样，从子串的角度是无法得出结论的，但是从字母的角度就会更轻松。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define ull unsigned long long
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
#define endl '\n'
const ll N = 2e5 + 5;
ll n;
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		map<char,ll>mp;
		cin >> n;
		for(ll i = 1; i <= 2 * n; i ++ )
		{
			string s; 
			cin >> s;
			for(auto i : s)
			{
				mp[i] ++ ;
			}
		}
		string s; 
		cin >> s;
		for(auto i : s)
		{
			mp[i] ++ ;
		}
		char ans;
		for(auto i : mp)
		{
			if(i.second % 2)
			{
				ans = i.first;
				break;
			}
		}
		cout << ans << endl;
	}
}
```