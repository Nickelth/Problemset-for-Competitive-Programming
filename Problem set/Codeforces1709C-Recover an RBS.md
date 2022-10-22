[题目链接](https://codeforces.com/problemset/problem/1709/C)

题目大意：

给出一个括号序列，其中有些括号被替换成了问号，问是否有一种唯一的方式，可以将问好替换成括号，且所得到的括号序列合法.

题解：

首先考虑如何得到一个一定合法的括号序列.

由于左括号的数量是所有括号数量的一半，因此我们可以算出需要补充多少左括号和多少右括号.

我们可以贪心地做，即让前若干个问号替换成左括号，后面的问号全替换成右括号.

原因是，判定括号的过程可以看作维护一个变量的过程，遇到左括号加一，遇到右括号减一，到末尾变量为零.

因此，由于题目保证给出的序列至少可以还原出一种合法的括号序列，我们先让该变量最大化，也就是先补充足够数量的左括号，然后再将剩下的问号替换为右括号，这样一定可以得到一个合法的序列.

那么，如何找到第二个合法的序列？

一种思路是，在上面这个合法序列的基础上，做影响最小的改动，即，调换最后一个被替换的左括号和第一个被替换的右括号，并检查所得到的序列是不是合法的括号序列.

那么，有没有可能，虽然调换这两个位置不能构成合法的序列，但是调换其他位置可以呢？

我们定义一个位置 $i$ 的 $balance$ 值为，从字符串开头开始，遇到左括号加一，遇到右括号减一，一直到达位置 $i$ 的值.

当我们调换了最后一个被替换的左括号和第一个被替换的右括号，二者中间所有位置的 $balance$ 值都减了 $2$ ，显然，调换其他任意两个左右括号位置，所导致 $balance$ 减少的区间都包括这个区间且长度更长，因此不会被调换这两者更优.

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
string s;
int check(string str)
{
	ll cnt = 0;
	for(auto i : str)
	{
		if(i == '(')
		{
			cnt ++ ;
		}
		else
		{
			cnt -- ;
		}
		if(cnt < 0)return 0;
	}
	return (cnt == 0);
}
void solve()
{
	cin >> s;
	ll cnt = 0;
	for(auto i : s)
	{
		if(i == '(')
		{
			cnt ++ ;
		}
	}
	
	cnt = s.length() / 2 - cnt ;
	ll pos1 = -1, pos2 = -1;
	for(ll i = 0; i < s.length(); i ++ )
	{
		if(s[i] == '?')
		{
			if(cnt)
			{
				s[i] = '(';
				cnt -- ;
				pos1 = max(pos1, i);
			}
			else
			{
				s[i] = ')';
				if(pos2 == -1)pos2 = i;
			}
			
		}

	}
	
	if(pos1 != -1 && pos2 != -1)
	{
		swap(s[pos1], s[pos2]);

		if(check(s))
		{
			cout << "NO" << endl;
		}
		else
		{
			cout << "YES" << endl;
		}
	}
	else
	{
		cout << "YES" << endl;
	}
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